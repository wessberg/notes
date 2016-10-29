## (C#) Lambdas, Extensions and LINQ
> [PrC#6]: chapters 9 and 13.
> [C#P2]: chapters 12.22, 12.23, 12.24, 12.25

## .NET Core

Cross-platform version of .NET (works on macOS, Linux & Windows).

### CLI
name: `dotnet`
Make sure that the executable is in your path.

### CLI Commands
#### `dotnet new`
Creates a new .NET project with a `Program.cs` file (a basic 'Hello world!' example).

#### `dotnet restore`
Fetches dependencies from NuGET.

#### `dotnet run`
Compiles and runs a .NET application.

#### `dotnet test`
Runs all the project's tests and prints the results on the command line.

## `delegate` keyword.
Kinda like abstract class methods.
Say you declare the following field on your class:
```c#
public delegate void Act(object o);
```
You could then later in your code state that a variable has the type `Act`, but that would require you to properly implement the signature of the `Act` delegate.
This would be illegal:
`Act print = delegate(string str) { Console.WriteLine(str); }`
Reason: argument 1 must be an object.

This would be illegal too:
`Act print = delegate(object o) { return o; }`
Reason: delegate method must return `void`.

This, however, would be legal:
`Act print = delegate(object o) { Console.WriteLine(o); }`.

## Generic `Func` type
`Func` is a generic `delegate`. The signature is
`Func<T, TResult>` (it can have up to 16 arguments where the last one is the type of the return value.)
That means that `Func<int, int, int>` means *Give me two integers and make me return an integer*. For instance, the following method is a valid usage of `Func` with lambdas:
```c#
Func<int, int, int> subtract = (x1, y1) => x1 - y1;
```

## Generic `Action` type
A specialized version of `Func` that says `Give me something and return void`. Its basically a more semantic way to write methods that doesn't return anything.
For instance,
```c#
Action<string> Print = str => Console.WriteLine(str);
```

## Generic `Predicate` type
A specialized version of `Func` that says `Give me something and return a boolean`. Its basically a more semantic way to write methods that returns boolean values. For instance,
```c#
Predicate<int> isEven = x => x % 2 == 0;
```

## Anonymous types.
Pretty cool stuff. Say you write the following C# code:
```c#
var hero = new { Name = "Batman", City = "Gotham City" };
```

What you'll get is an anonymous type: `{name: string, city: string}` and the instance `hero` with the respectful values `"Batman"` and `"Gotham City"`.

#### Object destructuring in Anonymous types.
Say you have the following variables:
```c#
var Name = "Superman";
var City = "Metropolis";
```

C# can infer the field names and values directly from the local variables via object destructuring:
```c#
var hero = new { Name, City}
```

#### Gotchas
**All properties of anonymous types are read-only.**
You can't do this `hero.Name = "foo";` after declaring the anonymous types.

## Extensions
Sometimes, you don't want to subclass something to give it new functionality. For instance, `IEnumerables` have a fixed set of instance methods available. However, with extensions, you can add more instance methods to a class. For instance, say you'd like to extend the `IEnumerable` interface with a `Print` method. You would then declare an static class with a generic method `Print<T>`:
```c#
public static class IEnumerableExtensions {

	public static void Print<T>(this IEnumerable<T> items) {
		throw new NotImplementedException();
	}
}
```
The `this` keyword inside the first argument refers to the object that we are extending.

You would then be able to call the `.print()` method on all `IEnumerables`.

Thats pretty awesome. You'd then be able to fit it inside functional chains like:
```c#
str
	.ToLower()
	.MakeSuperAwesome()
	.NoNotThatAwesome()
	.ABitMoreAwesomeNow()
	.Perfect()
```

## LINQ (Language-Integrated-Query)
The LINQ framework for C## adds a bunch of extension methods that makes it support functional queries like we know from functional languages such as SQL.
It allows for stuff like:
```c#
var result = collection
	.where(x => x.Value > 2)
	.OrderBy(x.Name)
	.TakeFirst(2)
```

LINQ expressions gets evaluated every time you enumerate on them. Say you have the following expression:
```c#
var result = repo.Numbers.Where(x => x >= 10);

repo.Numbers.add(10);
Console.WriteLine(result);
```

'result' would then also print the value we added after declaring the variable. It is a *live-collection* in the sense that it gets evaluated once every time you enumerate it.

If you instead want a fixed-in-time representation of the query, call 'ToArray', 'ToList' or 'ToDictionary' on the result.

LINQ is tightly integrated with C#. So much that you can write SQL-like syntax empowered by the framework to do your functional queries.

The following is valid c# code:
```c#
var heroesThatStartsWithB =
	from hero in heroes
	where hero.AlterEgo.StartsWith("B")
	select hero;
```

That would return all heroes that starts with *B* in a SQL-like way. The only difference being that the `from` and `select` statements have switched places.
