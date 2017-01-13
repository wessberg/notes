# (C#) Delegates, Lambdas, Extensions and LINQ
> [PrC#6]: chapters 9 and 13.
> [C#P2]: chapters 12.22, 12.23, 12.24, 12.25

### Delegates
Delegates are **addresses to methods**. Its like function pointers from C++ except not to a memory location but to a complex, type-safe classes that define the return types and types of parameters.

Delegates are used **when you want to pass methods around to other methods**.

What makes delegates particularly smart is that you can pass lambda expressions directly to a parameter that is of a delegate type.

So, note-to-self, its like the following type declaration in flow:
```javascript
aMethod (func: (someArg: string) => number): void;
```

Where we'd simply say: *"the first argument must be a function that takes a string and returns a number"*.

Back to C#:

Here's an example of how to declare a `delegate`:
```csharp
delegate void IntMethodInvoker(int x);
```

This indicates that each instance of this delegate can hold a reference to a method that takes one `int` parameter and returns `void`.

Another example could be a method that takes no arguments and returns an int:
```csharp
delegate int ReturnInt();
```

The delegate then becomes a **type**, meaning, its effectively a `class` with a single method. To use a delegate, you'd have to implement it:
```csharp
public class MyClass
	{
		delegate int ReturnInt(string str);
		public MyClass()
		{
			var method = new ReturnInt(Whatever);
		}

		private int Whatever(string str)
		{
			return 2;
		}
	}
```

Delegates in C# always has a one-parameter constructor, the parameter being the method to which the delegate refers. The methods signature must reflect the signature of the delegate.

#### An easier way of writing Delegates
In the example above, we could also have written the following:
```csharp
ReturnInt method = Whatever;
```

C# would then translate that to the previous example. Its a short-hand syntactical-sugar way of doing the same thing. This is called **Delegate inference**.

## Use cases
- **Sorting**: A sorting method can accept a comparer method (a method that will be responsible for the comparisons). This allows the user of a library to easily define his/her own sorting.

- **Events**: When an event is raised, the runtime needs to know what method should be executed (like `addEventListener(name, handler)` in Javascript, where handler is a delegate method).

### `Action<T>` and `Func<T>` delegates
C# comes with some predefined delegates that you should use when you can.

### Action<T>
An `Action` is for calling a method that returns `void`. It is generic, so you can go ahead and have fun with up to 16 parameters (including one without parameters).

### Func<T>
A `Func` is similar to `Action`, except it also has a `return` type: `Func<in T, out TResult>`.

## Multicast Delegates
It is possible for a delegate to wrap more than one method. This is used as a *multicast delegate*.

**When a multicast delegate is called, it successively calls each method in order.**

The delegate signature should return `void`, otherwise you would only get the result of the last method invoked by the delegate.

Via some truly black Microsoft-Magic, you can add or remove method calls from the delegate with the `+=` and `-=` operators:
```csharp
Action<double> operations = MathOperations.MultiplyByTwo;
operations += MathOperations.Square;
```

So, then it works like `yield` (at least to me) where, each time you invoke the `operations` delegate method, the "next" delegate function is called.

For instance:
```csharp
public class MyClass
	{
		public MyClass()
		{
			// When an action should take no arguments,
			// obviously it should not be made generic.
			Action operations = () => WriteLine("Foo!");
			operations += () => WriteLine("Bar!");

			operations(); // Prints "Foo!";
			operations(); // Prints "Bar!";
		}
	}
```

**IMPORTANT! The order in which chained methods will be called with multicast delegates is arbitrary! Do not rely on expecting it to be ordered!**

## Anonymous methods
Instead of passing a reference to a method, you can pass an *anonymous method* (not to be confused with a lambda):
```csharp
Action action = delegate()
{
	// Anonymous method body
}
```

So, you simply *invoke* the delegate keyword and put the method body inside the curly brackets. If the method should take arguments, put 'em inside the invocation parentheses.

Obviously, this is a lot prettier with lambdas:

### Lambda expressions
Lambda expressions are directly related to delegates.
Lambdas can be used whenever you have a delegate parameter type as an inline method.

For instance:
```csharp
public class MyClass
	{
		delegate void OnReady();

		public MyClass ()
		{
			DoCoolStuff(() =>
			{
				WriteLine("I am called every second!");
			});
		}

		async Task DoCoolStuff(OnReady callback)
		{
			while (true)
			{
				await Task.Delay(1000);
				callback();
			}

		}
	}
```

Notice how the `DoCoolStuff` method takes a delegate type which allows you to pass a method to the method as if it was *first-class citizen*. Notice also how I simply passed a lambda expression to the method invocation.

Lambdas in C# really is a beautiful thing, but it is *exactly* like in ES2015, so I'm not going to write notes on it.

## Closures
Again, a beautiful thing. Allows you to access variables outside the block of the lambda expression (I'd rather say outside the current scope of execution).

Obviously this has some implications - it means that the variables referenced inside the lambda expression won't be garbage collected (they often shouldn't anyway.

I guess I intuitively understand Closures from Javascript and don't feel like writing that much about it.

## Events
Events are based on delegates.
To define an event, use the `event` keyword.
For instance,
```csharp
public event EventHandler<SomeEventInfoArgs> SomeEventType;

// Later on
SomeEventType.Invoke(this, new SomeEventInfoArgs(something));
```

Here, the event that is offered is a `SomeEventType` event of type `EventHandler<SomeEventInfoArgs>`.

To invoke an event, you must pass the sender of the event (typically `this`, but not necessarily) as well as an object that derives from `EventArgs`.

## Weak Events
With events, the publisher and listener are directly connected. This can be a problem with garbage collection.

Use the weak event pattern and the `WeakEventManager<T>` as an intermediary between the publishers and listeners to get around this (it seems like this is specific for Windows as it is part of `System.Windows`).

# Language Integrated Query (LINQ)
