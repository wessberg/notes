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

			operations(); // Prints "Foo!" followed by "Bar!";
		}
	}
```

**IMPORTANT! The order in which chained methods will be called with multicast delegates is arbitrary! Do not rely on expecting it to be ordered!**

## Anonymous methods
Instead of passing a reference to a method, you can pass an *anonymous method* (not to be confused with a lambda):
```csharp
Action action = delegate
{
	// Anonymous method body
}

// or with arguments
Action action2 = delegate(string arg1)
{

}
```
You can't use `goto`, `continue`, `yield` or `break` in there.

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
LINQ integrates query syntax inside C#, making it possible to access different data sources with the same syntax.

## LINQ Query
Let's kick it off with an example:
```csharp
var query = from something in SomeCollection
						where something.Name == "Mickey"
						select something;
```

This selects all the elements in `SomeCollection` with the name "Mickey".

### Query contents
The query expression must begin with a `from` clause and end with a `select` or `group` clause.

In between you can use `where`, `orderby`, `join`, `let` and additional `from` clauses.

## LINQ Extension methods
The compiler converts the LINQ query to invoke method calls instead of the LINQ query.

Right from the top, LINQ offers various extension methods for the `IEnumerable<T>` interface, so you can use the LINQ query across any collection that implements this interface.

Extension methods, of course, make it possible to write a method to a class that doesn't already offer the method at first without subclassing it and referencing it instead.

**Extension methods can *not* access private members of the type it extends.**

Anyway, you can easily include the LINQ extension methods to classes that implement `IEnumerable` by simply importing the namespace `System.Linq` where you want to use it.

This brings some nice methods like `Where` and `Select` to collections.

But let's get back to the LINQ query syntax:

### Deferred query execution
It may look like the query takes place at the assignment in the example above where I queried all names starting with "Mickey". **but it does not! Instead, the query is performed as soon as the query is accessed. So, if you don't want the query to run each time you refer to the variable you stored it in, you must convert it to an array or a list or something like it first. by calling `ToArray` or `ToList` on the query**.

You could say that it is a "Live" collection if you don't convert it. To do so, check out this example:
```csharp
var query = (from n in names
						where n.StartsWith("John")
						select n).ToList();
```

## Standard Query Operators
Some of the most popular LINQ extension methods, but certainly not all, are supported by the LINQ query syntax.

The rest of them are available with the `Enumerable` class as extension methods.

It's pretty much [Lodash](https://lodash.com/docs/4.17.4#countBy)'s collection methods, except for C#.

`Where` was one example. But there is also `Skip`, `TakeWhile`, `Any`, `All`, `Zip`, `Union`, `Distinct` and lots of other goodies.

## Filtering
Lets see how to filter stuff with queries with the `where` clause.

It works intuitively, so there is not that much to add, but here is an example:
```csharp
var query = from p in People
						where p.Age > 13 &&
						(p.name.StartsWith("John") || p.name.EndsWith("Johnson"))
						select p;
```

Of course you could call `Where` directly without the LINQ syntax:
```csharp
Where(p => p.Age > 13 && (p.name.StartsWith("John") || p.name.EndsWith("Johnson"))).Select(r => r);
```

## Filtering with index
This can not be done with the LINQ syntax, but it can be done with the `Where` extension method. It is the same, except the delegate lambda method gets passed the index as the second argument:
```csharp
Where((p, index) => p.Age > 13 && index % 2 != 0);
```
That would only select people on even index positions in the collection.

## Compound from
If you need to do a filter based on a member of the object that itself is a sequence, you can use a compound from:
```csharp
var query = from p in People
from h in p.Hobbies
where h == "Dancing"
select p;
```

This will select all people with "Dancing" as a hobby.

## Sorting
The `orderby` clause can do that! You can follow up with `descending` or `ascending` as you like:
```csharp
var query = from p in People
						where p.Age > 13
						orderby p.Popularity descending
						select p;
```
This will list all the people that are older than 13 by their popularity.

## Sorting on multiple keys.
Sure! Just write `orderby <property1>, <property2>, ... <propertyN>`.

## Grouping
To group results based on a key value, use the `group` clause.

You write `group <property> by <something> into <newIdentifier>` where `<newIdentifier>` is an identifier for the group that can be used later to access the group result information.

The group will then have a `Count()` method and `key` property.

For instance:
```csharp
var query = from p in People
group p by p.FavoriteDish into g
orderby g.Count() descending, g.Key
select new {
	FavoriteDish = g.Key,
	Count = g.Count()
}

/*Looping through and printing these would become:
Cheeseburger 10
Lasagna 3
Burrito 1
etc...
*/
```

You wouldn't have to create the anonymous type in the select statement, you *could* also just select the group, `g`.

## Variables within the LINQ Query
You can use the `let` keyword inside LINQ query syntax to define variables within it:
```csharp
var query = from p in People
group p by p.FavoriteDish into g
let Count = g.Count()
orderby count descending, g.Key
select new {
	FavoriteDish = g.Key,
	Count
}
```

This can break up complexity a bit, but maybe more importantly allow you to not call the same methods several times for stuff you do need to retrieve from a method (or, for that matter, a getter) several times.

## Inner Joins
You can even do joins with the `join` keyword with the LINQ query syntax. It requires you to have two queries:
```csharp
var people = from p in People
where p.Age > 13
select p;

var mice = from m in Mice
join p in people on m.Owner equals p.Name
select new
{
	m.LengthOfTail,
	m.Owner,
	OwnerHobbies = p.Hobbies
}
```

## Left Outer Joins
If you want to select all records in the left sequence even when no match is found in the right sequence, you can use a Left Outer Join:
```csharp
var mice = from m in Mice
join p in people on m.Owner equals p.Name into mt
from p in mt.DefaultIfEmpty()
select new
{
	m.LengthOfTail,
	m.Owner,
	OwnerHobbies = p == null ? "The owner is not a person. So sad! We can't show hobbies for a nobody" : Hobbies
}
```

The `DefaultIfEmpty()` method makes sure that the default value for the right side is defined to the `default` value (remember: for value types this would be 0. For reference types, this would be `null`).

The book goes on to describe Group Joins, but I think I get the idea.

## Set operations
The extension methods `Distinct`, `Union`, `Intersect` and `Except` are set operations.

## Zip
The `Zip` method enables you to merge two related sequences into one with a predicate function.

## Partitioning
`Take` and `Skip` can be used for easy paging. For instance, skip the first 10 records or/and only take 3 results in total.

## Aggregate operators
`Count`, `Sum`, `Min`, `Average` and `Aggregate` are all covered by LINQ, though not with matching keywords, instead the extension operators must be used.

## Generation operators
`Range`, `Empty` and `Repeat` are *not* extension methods, but instead normal static methods that return sequences.
They are pretty nice!
```csharp
var values = Enumerable.Range(1, 20);
```
Boom! It returns a `RangeEnumerator` that simply does a `yield return` with the values incremented.

The `Empty` method returns an iterator that does not return values. This can be used for parameters that require a collection for which you can pass an empty collection.

The `Repeat` method returns an iterator that returns the same value a specific number of times.

## Parallel LINQ (PLINQ)
Pretty hardcore. Allows you to split the work of queries across multiple threads that run concurrently (e.g. in parallel).

Even better, it pretty much works without any developer effort. simply call `AsParallel()` on the queried collection:
```csharp
var query = from p in People.AsParallel()
where p.Age > 13
select p;
```

## LINQ Providers
.NET includes several of them. One must implement the standard query operators for a specific data source. But, LINQ providers may implement more extension methods than are defined by LINQ.

For instance, there are `System.Xml.Linq` which are particularly useful with XML.
