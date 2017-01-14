# Dependency Injection, Testable Entity Framework
> [Some guy named Martin Fowler](https://www.martinfowler.com/articles/injection.html)
>
> [Lecture slides (ITU students only)](https://learnit.itu.dk/pluginfile.php/163201/mod_resource/content/0/Lecture05.pdf)

## `using` statement
If you do this inside a method:
```csharp
using (Something something = new Something())
{
	something.DoSomething();
}
```

You ensure that the object is actually disposed as soon as it goes out of scope.

It is shorthand for actually wrapping the whole inner block in a try-finally block and always calling `Dispose` on the thing you are using.

## Dependency Injection
Based on the idea of the *inversion of control*.

The point is - instead of having the various components of your app depend on an implementation, instead let them depend on an interface. Then pass the actual implementation to them in the constructor, preferably using a so-called ServiceContainer (so that it "just works";).

So, you program to an interface, not an implementation.

Obviously, this has lots of benefits:
- You can very easily simply switch out the actual implementation, as long as it conforms to the same interface.

- Its very easily testable with stubs/mocks to isolate the thing you want to test by mocking some of the functionality from the test method (more on that later)

- It makes us think of our applications in terms of *modules*.

- It makes it easy to, for instance, change a dependent algorithm simply by passing another implementation to a module. For instance, If we found out that shell-sort was better than bubble-sort, as long as it conforms to the same interface, we could just give such an algorithm to a method. But *also*, if we suddenly felt like the sorting should be descending rather than ascending, we could just pass another implementation to a module.



## IoC (Inversion of Control)
Quite broad and imprecise. The name dependency injection is chosen to provide more precision. However, we do use the term *IoC Container* for Service Providers (which are responsible for injecting implementations of interfaces into classes that depends on them).

## IoC Container
An IoC container can have various lifetimes:

- Transient (every time).
- Scoped (once per request).
- Singleton (once).

### Forms of Dependency Injection
The basic idea is to have a separate object, an *assembler*, that populates a field in classes that depends on an implementation of something (usually its passed to the constructor).

So, the relevant class(s) have references to only the interface.

The interface have no references to anything.

The implementation has a reference to the interface.

But then the assembler have references to:
- The interface
- An implementation of it
- The class(s) that depends on the implementation.

So, if we want to change the implementation, we only need to make sure that the assembler has the one we want.

## Styles of Dependency Injection
### Constructor Injection
Okay, so the point is that the IOC Container uses a constructor to decide how to inject an implementation into a specific class. This means that the constructor of such a class **must** include everything it needs injected:
```csharp
public class MyClass
{
	private readonly ISomeDependency _someDependency;
	private readonly ISomeOtherDependency _someOtherDependency;

	public MyClass (ISomeDependency someDependency, ISomeOtherDependency someOtherDependency)
	{
		_someDependency = someDependency;
		_someOtherDependency = someOtherDependency;
	}
}
```

It is then in the setup of the IOCContainer that you declare which implementation to inject for each interface:
```csharp
public void ConfigureServices(IServiceCollection services)
{
	services.AddScoped<ISomeDependency, SomeImplementation>();
	services.AddScoped<ISomeOtherDependency, SomeOtherImplementation>();
}
```

### Setter Injection
Instead of giving the implementation(s) to the constructor, setters must be provided through which the dependencies will be injected:
```csharp
public ISomeDependency someDependency { private get; set; }
```

One great thing is that it allows you to use your constructor more freely in regards to passed arguments.

### Interface Injection
Here, classes that should have stuff injected must implement an interface that states which dependencies to inject:
```csharp
public interface IInjectSomeImplementation
{
	void injectSomeImplementation(ISomeImplementation someImplementation);
}

// And then in a class
public class SomeClass: IInjectSomeImplementation
{
	private ISomeImplementation someImplementation;

	public void injectSomeImplementation(ISomeImplementation someImplementation)
	{
		this.someImplementation = someImplementation;
	}
}
```

## Using a Service Locator
Another way to break the traditional dependency between a class and an implementation of a dependent functionality is by using a so-called *Service Locator*.

The difference is that a Service Locator has getter methods (or properties) for all concrete instances of implementations of interfaces. So, a class that might want to use something can just depend on the interface as well as the service locator and then get it from that.
For instance:
```csharp
ISomeDependency someDependency = ServiceLocator.GetImplementationForISomeDependency();
```

And then it would have that to play with.
It does require the class to have one more dependency (the Service Locator), but its more on an ad-hoc basis. Now, classes will declaratively ask for a reference to an implementation.

## `IDisposable`
Be very aware to implement `IDisposable` on stuff that takes dependency injections:
```csharp
public class Something: IDisposable
{
	// implementation
	// ...

	public void Dispose()
	{
		_service.Dispose();
	}
}
```

Because with dependency injection, you might end up in a situation where an instance will never be garbage collected even though you don't need it anymore anywhere.

### Best practices
- Use constructor injection
- Use Adaptor to enable interface if needed (for instance, for sealed classes).
- Implement IDisposable.
- Use an IoC container

## Unit Testing is awesome with Dependency Injection!

### Disclaimer
- Don't test against a live database or web service.
- Use mocks or stubs instead.

### Stub Testing
A stub is a dummy "placeholder" for real functionality. For instance, when you unit test your something that requires a database call, you don't want your code to *actually* invoke methods on the live database. Instead, you can provide *stubs* that "fake" the database functionality - so that you can focus on the thing you want to isolate.

Because, if you can fake the database transactions, you *know* that it always works correctly - you can isolate the specific thing you want to test.

For instance given this stub:
```csharp
public class DatabaseUpdateFalseStub: IFooService
{
	public bool Update(Foo foo)
	{
		return false;
	}

	public void Dispose
	{
	}

}
```

You can now use this in place of the database in a test where you are interested in knowing *"what happens with this thingy if the database returns false on an `Update` call?"*:
```csharp
public class Tests
{
	[Fact]
	public void Something_Blabla_returns_false()
	{
		IFooService service = new DatabaseUpdateFalseStub();

		using (var something = new Something(service))
		{
			var result = worker.DoWork(new Something());
			Assert.False(result);
		}
	}
}
```

Here, we were able to pass in the Stub as the service provider. **And that is what makes dependency injection so great for testing!**. The fact that you can pass in stubs to mask specific functionality and to test better.

### Mock testing
Stubs are great - *but* - you must write a lot of code to create Stubs. You must write a new class for *each* different Stub. That will get tiresome very quickly. And it will end up in an enormous bank of stubs!

Mocks to the rescue! Mocks are smarter kinds of stubs. Use the Moq framework.

It uses Reflection to create a new type for you on runtime which by default is just a new instance of the interface you give it with default values.

You get it by `mock.Object`:
```csharp
[Fact]
public void DoSomething_blabla_blabla_returns_false()
{
	var mock = new Mock<IFooService>();
	IFooService service = mock.Object;

	// Now you have an auto-generated instance of IFooService!
	using (var something = new Something(service))
	{
		var result = worker.DoWork(new Something());
		Assert.False(result);
	}
}
```

The reason why this example works is because, as I wrote, Moq sets default values for everything. And so it knows that `Update` must return a `boolean`. Since the default value of `boolean` is `false`, it works. If it should return an `int`, for instance, the default value would have been 0.

#### Changing Mock values (so you don't depend on default values)

Here's how:
```csharp
var mock = new Mock<IFooService>();
mock.Setup(m => m.Update(It.IsAny<Foo>())).Returns(true);
```

So, it says *"Whenever Update is called with ANY instance of Foo, return true."*

## Testing Entity Framework
First of all, you should *not* test third-party code. Assume that it works. The Entity Framework probably does :-)

Anyway, you can add the `Microsoft.EntityFrameworkCore.InMemory` package which allows you to hold the database in memory (which will be great for unit testing).
