## Exam Question C01
*Explain how parallel and asynchronous processes are handled in the .NET architecture and C#. Explain how the Task Parallel Library and the async and await keywords solve problems related to parallel and asynchronous programming.*

### Presentation
Asynchronous programming is incredibly important for a great user experience.
With *asynchronous programming*, the calling thread is not blocked.

This is especially important when it comes to the UI thread for which long running tasks such as fetching things from the network should still be responsive.

But it can also be complex to wrap your head around.

For instance, imagine doing async work using what's called the Asynchronous pattern where you pass a callback delegate function to
an asynchronous method call:
```csharp
	void DoStuff()
		{
			DownloadAsync(str =>
			{
				WriteLine(str);

				DoSomethingWithStringAsync(str, () =>
				{
					WriteLine("Finished!");
				});
			});

			WriteLine("Foo");
		}
```
This leads to callback-hell.
Also, it is confusing for developers who might expect the execution to happen in an asynchronous fashion. Obviously, here
the `WriteLine` statement outside the async body is called first.

Many developers have chosen to always go with the synchronous version of a method if such one existed, simply to avid confusing and mindbuggling code as that.

Now lets see how this code could have been written instead:
```csharp
static async void DoStuff()
		{
			var str = await DownloadAsync();
			WriteLine(str);
			await DoSomethingWithStringAsync(str);
			WriteLine("Finished!");
			WriteLine("Foo");
		}
```
This looks completely identical to how you would have written the code synchronously.
This is called the Task-Based Asynchronous Pattern, and it is based on the idea that there is a need to allow long-running activities to occur without blocking the thread, but also to not lose the current execution context.

So this is great, but often times we need to do stuff concurrently. For instance, we might want to utilize more than one thread to perform parallel work. And by relying on `async/await` for each invocation, we would have sequential operation.

C# and .NET has the `Task` class which contains information about the task created and allows waiting for its completion by passing a delegate to its `ContinueWith` method. This is actually what the compiler converts an await statement to if a Task returns a value.

But Tasks are much more powerful than that. They can support task parallelism which means that we can perform multiple tasks in parallel.

We can either use its' `WhenAll` or `WhenAny` methods which are awaitable and allows concurrent operation of two Asynchronous methods. And whats great about it is that we don't have to perform any checks for whether or not both operations have successfully completed:
```csharp
await Task.WhenAll(SomethingAsync1, SomethingAsync2);
// Proceed
```

`Task` manages a thread pool which means that it is relatively cheap to delegate work to other threads with it, rather than having to create a new one.

But we can also achieve *data parallelism* with the `Parallel` class. When we talk about data parallelism, we usually refer to scenarios in which the same operation is performed concurrently on elements in a collection such as an array.

In particular, it has a `Parallel` variant of a `For` and `ForEach` loop.

Obviously, in such situations, we need to be really careful about race conditions which happens when two or more threads write to the same data concurrently.
This can be achieved with locking the blocks of code in which we write to resources that would otherwise be accessed by another thread and thus handling it as a critical section. We could also make use of Concurrent collections which are thread-safe by design.


## Exam Question C02:
*Explain the principles of Design Patterns and describe in detail the Bridge, Adapter, Façade, and Strategy patterns including how they can be implemented in C#.*

Design patterns are repeatable solutions to commonly occurring problems in object design. It isn't a finished design that can be transformed directly into code, but rather a template for how to solve a problem that can be used in many different situations.

Design patterns has had lots of field tests. We already know their pros and cons and are able to effectively communicate their value to other developers, who will know them already and be familiar with them. And, our object design will probably become stable earlier since reusing design patterns helps to prevent issues before they arise.

What many of these design patterns have in common is also an indirection communication serving the purpose of loosing up coupling between individual components which results in a more testable, maintainable system.

### Kinds of design patterns
We differentiate between 3 kinds:
- **Creational design patterns**: These has to do with the instantiation of instances of objects. *How* we get ahold of objects.
	-	Factory
	- Abstract Factory

- **Structural design patterns**: These identifies ways to realize relationships between objects.
	-	Adapter
	- Facade
	- Decorator
	- Bridge
	- Proxy

- **Behavioral design patterns**: These has to do with how the objects communicate.

### Adapter pattern
<img src="assets/adapter_pattern.png" />
**STRUCTURAL DESIGN PATTERN**

The Adapter Design Pattern converts the interface of a (typically legacy) component into a different interface expected by the client, so that the client and the legacy class can work together without changes.

This is useful in scenarios where the class is sealed  (and thus can't be derived from) or is coming from another assembly (class library, .dll file).

To implement this in C#, we would first of all obviously write the actual interface that the client expects and then implement it with an Adapter class. When the client calls a method on it, the Adapter would then do the appropriate operations on the Adaptee and return the expected results to the client:
```csharp
// This is the interface the client expects
public interface IExpectedService
{
	bool Something();
}

// This is an implementation of that interface. Dependency injected, of course!
public class ExpectedAdapter: IExpectedService
{
	// Dependency injected (for extra points)
	private readonly IOldService _oldService;
	public ExpectedAdapter(IOldService oldService)
	{
		_oldService = oldService;
	}

	// This method wraps the old implementation.
	public bool Something ()
	{
		try {
			return _oldService.SomeOldThing() > 0;
		}
		catch (SomeException)
		{
			return false;
		}
	}
}
```

### Façade pattern
<img src="assets/facade_pattern.png" />
**STRUCTURAL DESIGN PATTERN**
It can be used to simplify the use of a system by providing a unified interface for a group of various functionalities from a multitude of **related** interfaces/classes.

This can be very useful when some complex functionality is split into several classes but they all work together against the same goal. For instance, lets say we want to want to handle an image file that a user just uploaded to our site. We want to generate a multitude of versions in different resolutions, maybe also do facial recognition analysis as well as backing up to an amazon s3 instance, We could then have a `ImageManipulator` facade, maybe with a `Handle(Image image)` method which would then hold a reference to instances of all of these classes and call the respective methods on them:
```csharp
public class ImageManipulator
{
	private readonly IUploadService _uploadService;
	private readonly IResizerService _resizerService;
	private readonly IRobotService _robotService;

	public ImageManipulator(
	IUploadService uploadService,
	IResizerService resizerService,
	IRobotService robotService
	)
	{
		_uploadService = uploadService;
		_resizerService = resizerService;
		_robotService = robotService;
	}
	static async Task Handle(Image image)
	{
		await Task.WhenAll(
			_robotService.FindOn(image),
			_resizerService.Resize(image),
			_uploadService.Upload(image)
		);
	}
}
```

### Strategy pattern
**BEHAVIORAL DESIGN PATTERN**
If we require dynamic switching between two or more implementations on runtime and also want to be able to deal with possible future extensions, the strategy pattern is the way to go.

<img src="assets/strategy_pattern.png" />

This is useful if we are to choose between different *strategies*, hence the name, that conform to the same interface, but are complex enough that they are implemented separately. For instance, lets imagine that we could decide a sorting algorithm to go with at runtime. We'd then give the name of the required sorting algorithm such as mergesort or quicksort to a method, and it would then pick the algorithm to go with on runtime.

One way to do this in C# would be to a `Sorter` class with a `Sort` method that takes in a name for a sorting algorithm. It then uses a switch-statement to check for the given algorithm and calls the corresponding sort algorithm:
```csharp
public class Sorter
	{
		private readonly IMergeSorter _mergeSorter;
		private readonly IQuickSorter _quickSorter;
		public Sorter(IMergeSorter mergeSorter, IQuickSorter quickSorter)
		{
			_mergeSorter = mergeSorter;
			_quickSorter = quickSorter;
		}
		void Sort<T>(IEnumerable<T> arr, string strategy)
		{
			switch (strategy.ToLower())
			{
				case "mergesort":
					_mergeSorter.Sort();
					break;

				case "quicksort":
					_quickSorter.Sort();
					break;
				default:
					break;
			}
		}
	}
```

### Bridge pattern
<img src="assets/bridge_pattern.png" />
**STRUCTURAL DESIGN PATTERN**

The bridge pattern provides a solution for dynamically substituting multiple realizations of the same interface for different uses.

Here, again, we have different concrete implementations, and we want to be able to change them dynamically on run-time.

Its very similar to the Strategy pattern, except here the user doesn't know about (or at least determine) the actual implementation strategy.

This would be a great pattern to use if, for instance, you want to store your data in persistent storage and the actual storage strategy should dynamically change at runtime depending on your connectivity.

So what you would do is first of all implement both the online and offline persistent storage classes so that they conform to the exact same interface.

You'd then have the Bridge class hold an instance of that interface as a field, for instance given by dependency injection and let the client invoke methods directly on the bridge. That way, neither the bridge nor the client knows about the actual implementations:
```csharp
public interface ISync
{
	void Sync();
}

public class OnlineSync: ISync
{
	public void Sync()
	{
		// Implementation.
	}
}

public class OfflineSync: ISync
{
	public void Sync()
	{
		// Implementation.
	}
}

public interface IBridge
{
	void StartSync();
}

public class Bridge: IBridge
{
	private readonly ISync _sync;
	public Bridge (ISync sync)
	{
		_sync = sync;
	}

	public void StartSync()
	{
		_sync.Sync();
	}
}
```





## Exam Question C03:
*Explain the principles of Design Patterns and described in detail the Template method, Factory method, and Chain of Responsibility patterns including how they can be implemented in C#.*

Design patterns are repeatable solutions to commonly occurring problems in object design. It isn't a finished design that can be transformed directly into code, but rather a template for how to solve a problem that can be used in many different situations.

Design patterns has had lots of field tests. We already know their pros and cons and are able to effectively communicate their value to other developers, who will know them already and be familiar with them. And, our object design will probably become stable earlier since reusing design patterns helps to prevent issues before they arise.

What many of these design patterns have in common is also an indirection communication serving the purpose of loosing up coupling between individual components which results in a more testable, maintainable system.

### Kinds of design patterns
We differentiate between 3 kinds:
- **Creational design patterns**: These has to do with the instantiation of instances of objects. *How* we get ahold of objects.
	-	Factory
	- Abstract Factory

- **Structural design patterns**: These identifies ways to realize relationships between objects.
	-	Adapter
	- Facade
	- Decorator
	- Bridge
	- Proxy

- **Behavioral design patterns**: These has to do with how the objects communicate.

### Template Design method
<img src="assets/template_pattern.png" />
It defines the structure of an algorithm in one method - **but leaves some parts undefined.**

The actual implementation of the unspecified parts is contained in other methods which implementation is found in subclasses.

So, imagine for instance if we want to be able to generate some HTML markup, but where the body stays the same, but the header can change. With the template pattern, we would then declare an interface, `IHeadProvider` which can provide markup for the head part. We'd then also declare an interface `IMarkupProvider` which extends `IHeadProvider`, which can provide a body.

We would then implement an abstract base class, `MarkupProvider` which implements `IMarkupProvider` and declare the method for getting the head contents as `abstract`. Its important that it is abstract, because otherwise we would violate the Liskov Substitution principle of the SOLID principles. We would do so since the base class wouldn't be able to ever generate any head markup and thus we couldn't substitute a derived class with a base class.

It would then have a method for simply returning the body contents.

But then we'd subclass it with a `AwesomeProvider` which then implements the `IHeadProvider` interface. This one generates some awesome head contents and wraps the body inside of it.

The client would then simply expect an `IMarkupProvider` and call a `Provide` method on it. We could give it any implementation. For instance, if we want a less awesome header, we could just dependency inject that.

Here's some code for it:

```csharp
public interface IMarkupProvider: IHeadProvider
	{
		string ProvideBody();
	}

	public interface IHeadProvider
	{
		string Provide();
	}

	public abstract class MarkupProvider: IMarkupProvider
	{
		public string ProvideBody() => "<body></body>";
		public abstract string Provide();
	}

	public class AwesomeProvider : MarkupProvider
	{
		public override string Provide() => $"<head>{ProvideBody()}</head>";
	}


	public class Client
	{
		public static void Main(string[] args)
		{
			var client = new Client(new AwesomeProvider());
		}
		public Client(IHeadProvider markupProvider)
		{
			WriteLine(markupProvider.Provide());
		}
	}
```

### Chain of responsibilities pattern
<img src="assets/chain_of_responsibilities_pattern.png" />
**BEHAVIORAL DESIGN PATTERN**

This is a pattern that provides indirect communication between the sender of a request and the object that can actually handle it. by giving more than object a chance to handle the request.

A client submits a request to a *handler* which then decides if it can handle it. If it cannot, it will *pass on* the request to its successor, e.g. the next in the chain of responsibilities. And this continues until the request finally arrives at a concrete handler that can actually handle it. If none can (e.g. if the request arrives at the last link of the chain), that handler will know that the request can't be fulfilled and may reject it.

For instance, say we're on a business trip and request approval for something expensive, like taking a business partner out to dinner. We'd then pass that request to an *approver*, which could be a manager. If that manager cannot approve it (since its incredibly expensive), the manager then *pass on* the request to the vice president. If he cannot approve it either, it finally arrives at the CEO who gladly approves it.

To do this in C#, we would first create an interface called *IApprover*. We'd then make sure that the Manager, VicePresident and CEO also implement that interface.

And then we combine them in a Linked-List fashion where the manager is passed a reference to the VicePresident, and the VicePresident is passed a reference to the CEO.

I imagine they just pass a the request around to each other, and all of them simply return the value of their successor.

They all implement a method, `RequestApproval` which returns a boolean. If one can't approve the purchase, he simply return the result of calling the same method on the successor:

```csharp
public class Request
	{
		public int Amount { get; set; }
	}

	public interface IApprover
	{
		bool RequestApproval(Request request);
	}

	public class Requester
	{

		private readonly IApprover _approver;
		public Requester(IApprover approver)
		{
			_approver = approver;
		}

		public void Send(Request request)
		{
			WriteLine($"Is approved: {_approver.RequestApproval(request).ToString()}");
		}
	}

	public class Manager: IApprover
	{
		private readonly IApprover _successor;
		public Manager(IApprover successor)
		{
			_successor = successor;
		}
		public bool RequestApproval(Request request)
		{
			return request.Amount < 1000 || _successor.RequestApproval(request);
		}
	}

	public class CEO : IApprover
	{
		public bool RequestApproval(Request request)
		{
			return request.Amount < 2000;
		}
	}
```

### Factory pattern
<img src="assets/factory_pattern_uml_diagram.jpg" />
**CREATIONAL DESIGN PATTERN**

The factory pattern can create instances of objects without exposing the creation logic to the client. This also means that you could call, for instance `shapeFactory.CreateShape("circle")` to get a circle, without having to rely on all sorts of import statements.

There are several ways to actually implement this. You could obviously wrap a huge `switch` statement inside the `CreateShape` method and manually create and return a new instance of the appropriate object based on the given string. If none exist, you could return null.

But if you want this to be extensible, you can use **reflection** to actually have C# figure out of such a code exists and if so create a new instance and return it to the user. This works by looking into the assembly and figuring out if there actually exists a type with such a name:
```csharp
public class ShapeFactory
{
	public IShape CreateShape(string name)
	{
		var shapes = typeof(IShape).GetTypeInfo();
		// Via linq
		var shape = shapes.Assembly
			.GetTypes()
			.Where(t => t.Name.Equals(name))
			.FirstOrDefault();

		if (shape == null) return null;
		return Something.CreateInstance(type) as IShape;
	}
}
```





## Exam question C04:
*Explain the concepts of unit testing with examples in C#. You should cover concepts like mocks, stubs, and the dependency injection pattern.*

## What is Testing
Testing is the process of finding differences between the expected behavior specified by system models and the observed behavior of the implemented system.

## What is Unit Tests
Unit Tests are automated tests which in computer programming terms is a technique for testing individual bits and pieces of source code to determine if they act as intended. We want to perform very isolated testing here, even down to a specific branch of an object method.

### Stubs vs Live testing
In order to do so, we may need to "fill-in" some of the logic that the object under test depends on.

For instance, if the method needs to perform a call do the database to fetch some persistent data and then performs some transformation on it, it would depend on access to a database.

We could obviously just hook up a database connection and pass that on in the `Arrange` step of the test method.

But that brings several disadvantages:
- You'll be testing against a live environment.

- There will be network delays.

- You can't rule out the possibility that the database returns faulty data. It might have a broken implementation. In any case, if you want to ensure full isolation of test method, you need to be in control of the dependent objects as well.

So what you do, then, is to provide a *dummy object* or a stub which is simply a placeholder for the real implementation.

All we care about is that the stub returns the anticipated values when the method under test actually uses it. Also, by doing so, we can test the method in even more detail by altering the return values of the dependent functionality to see how the method responds to that.

All of a sudden you can ask questions such as *"What happens if the database returns false here?"* and easily write a test for it.

This, by the way, is a great example of the reason why programming to interfaces is so well-suited for unit testing. If the method under test depends on a specific implementation, we need to provide an instance of that implementation. But, if it depends on an interface, we can provide our own implementation of it with a stub.

### Mocks
The thing about stubs, though, is that they are immensely time-consuming to write. You may need to write hundreds of stubs, and especially if you do TDD, you will end up with much more stub classes than actual classes.

To avoid this, we have this great concept of Mocks. These are smarter in the sense that they can auto-provide implementations of given interfaces with default values for everything using Reflection. This allows you to provide an implementation directly from the test method.

But you can also directly affect the actual return values of the properties and methods of the mock simply by setting it up:
```csharp
var mock = new Mock<IFooService>();
mock.Setup(m => m.Update(It.IsAny<Foo>())).Returns(true);
```

Here we say: *"When I invoke the Update method on the IFooService and gives it ANY instance of Foo, it will return true"*.

And now we can pass an implementation of that interface to the object under test with the `mock.Object` property and see how the method behaves under these circumstances.

### Dependency Injection
But we can only do that because we're using dependency injection! If the object under test is responsible for setting up its own implementation dependencies, we won't be able to do this. The most obvious way to do it is simply by using constructor dependency injection; We simply pass-in the mock object. That's the TL;DR; of the concepts behind dependency injection.

Typically, outside a test environment we'd then use an Inversion-of-Control container to constructor-inject concrete implementations of interfaces to instances of classes.

### The Assert step
So anyway, in the last step, the *Assert* phase, we then compare the result of the operation with the expectation. For instance,
```csharp
var result = ObjectUnderTest(mock.Object);
Assert.AreEqual("Hello World", result);
```

### Full example
So let's say we want to test a class `Foo` which implements an interface of type `IFoo` and expects an instance of an `IFooService`:
```csharp
public interface IFoo
{
	string Bar();
}

public interface IFooService
{
	bool Baz();
}

public class FooService: IFooService
{
	public bool Baz()
	{
		return true;
	}
}

public class Foo: IFoo
{
	private readonly IFooService _fooService;

	public Foo (IFooService fooService)
	{
		_fooService = fooService;
	}

	public string Bar()
	{
		return _fooService.Baz() ? "A" : "B";
	}
}
```

So now, lets' write a test method for it using Mocks:
```csharp
[Fact]
public void Foo_when_FooService_returns_false_returns_B
{
	// Arrange
	var mock = new Mock<IFooService>();
	mock.Setup(m => m.Baz()).Returns(false);

	// Act
	var foo = new Foo(mock.Object);
	var result = foo.Bar();

	// Assert
	Assert.AreEqual("B", result);
}
```

So, as you can see, it now becomes incredibly easy to just switch out `false` with `true` in the Mock and Assert that the result is "A". This is why dependency injection and Mocks are fantastic together!











## Exam question C08:
*Describe data binding and the MVVM model. Explain how MVVM relates to events, the Command pattern and the Observer pattern. Compare this architecture to the ASP.NET MVC architecture.*

### Presentation
Alright, so let's dive into MVVM.
<img src="assets/mvvm_2.png" />
It's all about separation of concerns. The View part is XAML based with a bit of *code-behind* and is data-bound to the ViewModel. The ViewModel then holds the actual state and operations. It receives change notifications from the model, and so does the View through declarative data binding. It then updates the Model accordingly upon user interactions and what not.

### Advantages:
- It is very easy to simply test the logic behind the View (the ViewModel).

- It is very easy to just swap out the actual view (the XAML-thingy) which makes it perfect for switching technologies or delivering different view components to different devices (which is perfect for Microsoft's Universal Platform strategy).

- It provides separation of concerns in a similar fashion to MVC.

### Keep the Code-Behind small
In order to achieve the separation properties of MVVM, we have to keep to footprint of the code-behind file as minimal as possible. This also means that we shouldn't place any event handlers there. Instead, the ViewModel should decide which actions should be handled by implementing the `ICommand` interface. The View should then bind to these actions to be able to relay the actual handling of the actions to the ViewModel instead.

(### Difference from MVC
In MVC, The controller knows about the View and the Model.
It makes sure to update the view when the model changes. And that might happen as a result of a user interaction such as a button that was clicked or something like that.

In terms of ASP.NET MVC, the controller receives requests and responds to requests. In MVVM. Staying with that analogy, here the View receives the request, but it never actually handles it.)

In MVVM, the View knows about the ViewModel and the ViewModel knows about the Model, the model doesn't know anything about the ViewModel and the ViewModel doesn't know anything about the view. This on its own means that MVVM is less tightly coupled than MVC.

### Relation to the Observer Pattern
It is very much related to the Observer Pattern, but the abstraction is a bit different. Here, the View observes the ViewModel through the abstraction of data binding. It can, however, also directly manipulate the `ViewModel` through TwoWay binding, but let's get back to that.

When the ViewModel change, it will notify the view which will update accordingly.

The ViewModel observers the model as well. This means that if the model changes something that the ViewModel is observing, and the View happens to bind that property, it too will be updated.

### Relation to the Command Pattern
By having the View completely isolated from the Model, we make sure that all requests to update the state happens through an intermediary. For instance, instead of having a button click directly alter the state of the model, we instead handle the request in the ViewModel. This also enables us to, for instance, introduce undo/redo or logging at a later point, because now we have full control over the request before fulfilling it.
