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
