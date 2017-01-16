## Testing (in C#)
> None

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


### By the way
- Default access modifiers for methods is **private**.

## TDD
Test-Driven development is test-first driven development. Here, we
- Write an intentionally failing test.
- Run it
- Assert that it actually failed.
- Then we actually implement it.
- Run it
- Assert that it passed.

Test-Driven development has the obvious benefit that we get great code coverage by simply not writing any code we haven't tested. This makes life easier in the long run. When we at a later point in time decide to refactor something, it becomes easy to verify that nothing broke when we do regression testing.

Another benefit is that when we *don't* go with the test-first approach, sometimes the tests we write actually pass, even though the intended behavior doesn't correlate with the functional or nonfunctional requirements. This can be due to a simple fault in our code.

So by running the test we wrote before ever implementing the method, we enhance the probability of not running into these kind of behaviors.

# Unit Tests
## Conventions and rules

- The name of the test *class* should be name of the class it tests, suffixed with `Tests`. For instance, `FooTests`.

- All test methods **must** be public (otherwise the test runner can't see it).

- The name of the method should reflect what it does. It should be clear from the signature of the test method *why* the method failed.

- The class method should follow the mantra **"Given-When-Then"** and start with the name of the class under test. So, overall the form should be: `<ClassName>_Given_<Something>_<WhenSomething>_<DoesWhat>` For instance, `Person_given_Age_2_when_Age_is_already_set_returns_false` or `Main_when_run_prints_Hello_World`.

## Arrange, Act, Assert (3 A's)
Its a convention for setting up unit tests:
- *Arrange*: Here, you set up whatever needs to be set up before acting out the test. For instance, mocks or stubs.

- *Act*: Here, you actually call the method(s) under test.

- *Assert*: Here, you assert that the outcome corresponds to the expected one.

```csharp
[TestMethod]
public void Main_when_run_prints_Hello_World ()
{
	// Arrange

	// Act

	// Assert
}
```
