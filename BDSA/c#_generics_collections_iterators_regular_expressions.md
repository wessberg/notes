# Generics, Collections, Iterators and Regular Expressions
> [PrC#6]: chapters 6, 10, and 11.
>
> [C#P2]: chapters 13.12, 26, and 27.

### Boxing
The conversion from a value type to a reference type is known as *boxing*.

Boxing occurs automatically if a method requires an object as a parameter and a value type is passed.

### Unboxing
A boxed value type can be converted to a value type by using unboxing. With unboxing, the cast operator is required.

For instance:
```csharp
var list = new ArrayList();
list.Add(44); // Boxing - converts a value type to a reference type.

int num = (int)list[0]; // unboxing - converts a reference type to a value type.
```

**Boxing and unboxing have big performance impacts, especially when iterating through many items.**

## Generics
Generic types and methods provide a way to strengthen type checking at compile-time to avoid some type checks and casts at run-time and to improve run-time data representation, while at the same time making programs more expressive, reusable and readable.

**READ: The ability to have generic types and methods is known as parametric polymorphism**.

### Generic classes/structs/interfaces are not really classes/structs/interfaces on their own
Rather, they are mechanisms for constructing classes/structs/interfaces. This means that whenever something like new `GenericClass<int>();` is called somewhere in the code, the assembly will generate a variant of the class that takes ints.

These are called **constructed types**.



## Generics with value types
C# support generics with value types! For instance using the `System.Collections.Generic.List` class:
```csharp
var list = new List<int>();
list.Add(44); // No boxing!

int num = list[0]; // No unboxing, no cast needed.
```

That's pretty neat.

## Naming guidelines for Generics
- Prefix generic type names with the letter `T` (such as `public class MyList<TSomething>`).

- If the generic type can be replaced by any class because there's no special requirement, and only one generic type is used, simply use the character `T` as a generic type name: `public class MyList<T>`.

- If there is a special requirement for a generic type, or if two or more generic types are used, use descriptive names for the type names: `public class MyMap<TKey, TValue>`.

## `null` and generic types (and the `default` keyword)
It is **not** possible to assign `null` to generic types.
That's because generics also support value types which does *not* allow `null` values.

To circumvent this problem, you can use the `default` keyword. With it, `null` is assigned to reference types and 0 is assigned to numeric value types:
```csharp
// Inside a generic class
public void SomeMethod ()
{
	T something = default(T); // null or 0.
}
```

## Constraints on generic types with the `where` keyword
If you want to make sure that some method or property exists on a generic type, you can add a `where` class to the class declaration:
```csharp
csharp
```
public class SomeClass<T> where T: ISomeInterface
```
This enables you to constraint all generic instances of the class to only support types that implement the given interface.

### All uses of the `where` keyword with generics
<table>
	<tr>
		<td><strong>Constraint</strong></td>
		<td><strong>Description</strong></td>
	</tr>
	<tr>
		<td><code>where T: struct</code></td>
		<td>type T must be a value type</td>
	</tr>
	<tr>
		<td><code>where T: class</code></td>
		<td>type T must be a reference type</td>
	</tr>
	<tr>
		<td><code>where T: IFoo</code></td>
		<td>type T is required to implement interface IFoo</td>
	</tr>
	<tr>
		<td><code>where T: Foo</code></td>
		<td>type T is required to derive from base class Foo</td>
	</tr>
	<tr>
		<td><code>where T: new()</code></td>
		<td>type T must have a default constructor</td>
	</tr>
	<tr>
		<td><code>where T1: T2</code></td>
		<td>type T1 must derive from generic type T2</td>
	</tr>
</table>

#### Combining Constraints
Like anything else, `where` constraints can be combined with commas:
```csharp
public class MyClass<T> where T: IFoo, IBumBum
```
## Static members
Because C# prepares multiple standard classes under the hood in the assembly for generic classes, be aware that static members are actually duplicated between generic classes and thus can hold different values simultaneously:
```csharp
public class MyGenericClass<T>
{
	public static int x;
}

// Somewhere in your code.
MyGenericClass<string>.x = 4;
MyGenericClass<int>.x = 5;
WriteLine(MyGenericClass<string>.x); // Writes 4, not 5.
WriteLine(MyGenericClass<int>.x); // Writes 5.
```

## Covariance and Contra-variance
Covariance and contra-variance are terms that refer to the ability to use a less derived (less specific - contra-variant) or more derived type (more specific - covariant) than originally specified.

Covariance and contra-variance are used for the conversion of types with arguments and return types.

### Contra-variance
Contra-variance allows you to use a **less** derived type than the one specified.

### Covariance
Like you intuitively know it from polymorphism, covariance allows you to use a **more** derived type than the one specified.

## Covariance and Contra-variance in generics with `in` and `out`
Only works with reference types! So you need to box value types as reference types.

### Covariance in generics with `out`
For generic types, the `out` keyword specifies that the type parameter is covariant. For instance,
```csharp
interface ICovariant (out T) {}
class Test<T>: ICovariant<T> {}

// Somewhere in your code
ICovariant<Object> obj = new Test<Object>();
ICovariant<String> str = new Test<String>();

// Works since String derives from Object.
obj = str;
```

### Contra-variance in generics with `in`
For generic types, the `in` keyword specifies that the type parameter is contra-variant. For instance,
```csharp
interface IContraVariant (in T) {}
class Test<T>: IContraVariant<T> {}

// Somewhere in your code
IContraVariant<Object> obj = new Test<Object>();
IContraVariant<String> str = new Test<String>();

// Works since String derives from Object, even though // it might seem a bit weird.
str = obj;
```

## Generic structs
Structs can also be generic and behaves very similarly to generic classes except for the inheritance features.

## Nullable value types
All reference types are nullable by default. Value types can't be null, so for this, nullable structs can be postfixed with a `?` to make them nullable:
```csharp
int? num = null; // Ok!
```

## Generic methods
Works like anywhere else, **except you can have generic methods even on non-generic classes!**

## Overloading generic methods
You can add generics into the mix when it comes to overloading methods. You can declare several overloaded methods that take different generic types or none at all:
```csharp
public class SomeClass
{
	public void Foo () {}
	public void Foo<T> (T obj) {}
	public void Foo<T1, T2> (T1 obj1, T2 obj2) {}
}
```

That's pretty nice.

# Strings
### Immutable
Strings are actually immutable. When you manipulate the contents of a string, you are actually creating a new string, copying over the contents of the old string if necessary. For instance, the following of course works perfectly fine, but under-the-hood it is a pretty costly operation because of the immutability:
```csharp
string str1 = "Hello, ";
str += "World!";
```

This is because, for each time a string is declared, exactly the amount of memory needed to hold the string is allocated.

Also, since `string` is actually a reference type (with special powers), it must wait for the garbage collector to come along and clean the reference up to the old unused object (the one that *were* `str1` before the `+=` operation).

Imagine if you had to loop through a huge list and for each entry append something to a string or something like that. That would be very inefficient for C# strings.

So - if you use strings to do text processing extensively, your applications will run into severe performance problems.

### `StringBuilder`
The `System.Text.StringBuilder` class is here to the rescue! It offers efficient appending or removing text from strings but doesn't have all nearly as much goodies as strings.

`StringBuilder` strings allocate more memory than you actually need. You then have the option to indicate just how much memory it should allocate, and if you don't, the amount defaults to a value that varies according to the size of the string with which the `StringBuilder` instance is initialized.

Use it when you make repeated text substitutions (with the `Replace()` instance method)/additions/removals.

## String interpolation
Works by using the $ prefix for strings:
```csharp
string s1 = "World!";
string s2 = $"Hello, {s1}";
```

Compiler transpiles it and creates invocations to `String.Format` behind the curtain.


## Regular Expressions
Has a static method `Matches(text, pattern, [options])` that returns a `MatchCollection`.

When declaring a pattern for a regular expression, use string prefixed with "@" to ensure that nothing will be escaped by the C# compiler. For instance:
```csharp
const string pattern = @"ions\b";
```

The flags is given as *options* to the `Matches` method, for instance `RegexOptions.IgnoreCase`.

### Named capture groups
You can do so with the syntax `?<name>` at the beginning of a group. For instance,
```csharp
string pattern = @"(?<protocol>https?)somethingsomething";
```

## Collection interfaces and types
Here are the most important ones (with generics):
<table>
	<tr>
		<td><strong>Interface</strong></td>
		<td><strong>Description</strong></td>
	</tr>
	<tr>
		<td><code>IEnumerable<T></code></td>
		<td>Is required by the <code>foreach</code> statement. Defines the method <code>GetEnumerator</code>, which returns an enumerator that implements the <code>IEnumerator</code> interface.</td>
	</tr>
	<tr>
		<td><code>ICollection<T></code></td>
		<td>Can get the number of items in the collection with the <code>Count</code> property and copy the collection to an array with the <code>CopyTo()</code> method. You can add to, remove from and clear the collection.</td>
	</tr>
	<tr>
		<td><code>IList<T></code></td>
		<td>For lists where elements can be accessed from their position (via indexing). You can also insert or remove items from specific positions. Derives from <code>ICollection</code>.</td>
	</tr>
	<tr>
		<td><code>ISet<T></code></td>
		<td>Allow combining different sets into a union, getting the intersection of two sets and checking whether two sets overlap. Derives from <code>ICollection<T></code>.</td>
	</tr>
	<tr>
		<td><code>IDictionary<TKey, TValue></code></td>
		<td>A collection of key-value pairs. All keys and values can be accessed. Items can be accessed with an indexer for keys and items can be added or removed.</td>
	</tr>
	<tr>
		<td><code>ILookup<TKey, TValue></code></td>
		<td>Similar to <code>IDictionary</code> except multiple values can be associated with each key.</td>
	</tr>
	<tr>
		<td><code>IComparer<T></code></td>
		<td>Used to sort elements inside a collection with the <code>Compare</code> method.</td>
	</tr>
	<tr>
		<td><code>IEqualityComparer<T></code></td>
		<td>A comparer that can be used for keys in a dictionary. Objects can be compared for equality.</td>
	</tr>
</table>

## Enumerators and Enumerables

### The `IEnumerator<T>` interface
An enumerator enumerates (produces) a stream of elements, such as the elements of a collection.

A class or struct type that has a method `GetEnumerator` with return type `IEnumerator` or `IEnumerator<T>` can be used in a `foreach` statement.

### The `IEnumerable<T>` interface
An enumerable type is one that implements `IEnumerable<T>`. This means that it has a method `GetEnumerator` that can produce and enumerator.

## Lists
Resizable lists with the generic class `List<T>` which implements the `IList`, `ICollection`, `IEnumerable` interfaces.

### Resizing
Lists are autoresizing. For every resize, the capacity is doubled. As soon as a single element is added to the list, its capacity is extended to allow 4 elements.

You can set the capacity yourself if you want to:
```csharp
myList.Capacity = 20;
```

You can get the amount of items currently in the list with the `Count` property.

You can get rid of unneeded capacity if you know that the list won't be extended no more with the `TrimExcess` method.

## Collection initializers
You can initialize a collection pretty nicely:
```csharp
var myList = new List<int> {1, 2, 3};
// or
var myList = new List<int>() {1, 2, 3}
```

### Sorting lists
Uses the quick sort algorithm.
You can also give it a `Comparison<T>`, `IComparer<T>` or a range (`Int32`, `Int32`) followed by an `IComparer<T>`.

## Read-Only Collections
If you call the `AsReadOnly` method on, for instance, a `List`, it returns an object of type `ReadOnlyCollection<T>` where all methods that would change the collection throws exceptions.

## Queues
FIFO-ordered collections. Implemented with the `Queue<T>` class.

## Stacks
LIFO-ordered collections. Implemented with the `Stack<T>` class. It implements the `IEnumerable<T>` and `ICollection` interfaces.

## Linked Lists
Implemented with the `LinkedList<T>` class. As always, insertions anywhere are very fast with Linked Lists.

## Sorted Lists
The `SortedList<TKey, TValue>` collection will sort the elements based on a key. As you add elements, the list will stay sorted based on the key.

## Dictionaries
Enables accesses based on a key. Also known as hash tables or maps. Fast lookups based on keys.

### Dictionary initializers
Here's the syntax:
```csharp
var dict = new Dictionary<int, string>()
{
	[3] = "three",
	[7] = "seven"
};
```
Which adds two elements with keys 3 and 7 and values "three" and "seven".

### Key type
A type that is used as key in a dictionary *must* override the method `GetHashCode` of the `Object` class. All built-in types already do that.

The requirements for the implementation of that method are:
- The same object should always return the same value.
- Different objects can return the same value.
- It must not throw exceptions.
- It should use at least one instance field.
- It should not change during the lifetime of the object.

## Lookups
Supports more than one value per key. Implemented with `Lookup<TKey, TElement>`.

## Sorted Dictionaries
A binary search tree in which the items are sorted based on the key. The key *must* implement the interface `IComparable<TKey>`.

## Sets
There are two kinds of sets, `HashSet<T>` and `SortedSet<T>` that both implement the interface `ISet<T>`.

`HashSet<T>` contains an unordered list of distinct items and `SortedSet<T>` is ordered (duh).

## The `yield` statement and Iterators
The `yield` statement is used to define enumerators and enumerables in a convenient way.

Can only be used inside the body of a method or operator or the get-accessor of a property or indexer whose return type is `IEnumerator`/`IEnumerator<T>` or `IEnumerable`/`IEnumerable<T>`.

The body of this *iterator* method is then called an *iterator block*. The *yield type* of the iterator block is Object or T respectively.

A `yield` statement can not appear in an anonymous method.

A `yield` statement must have one of these two forms:
<code>yield return <em>expression;</em></code>
<code>yield break;</code>

### `yield return something`
It returns a value to a caller just as a normal `return` statement does, *but* it does not terminate the enclosing iterator block and does not execute any associated `finally` clauses.

**Instead, the execution of the iterator block can be resumed just after the `yield` statement, possibly causing the iterator block to execute more yield statements**.

### Example
```csharp
public IEnumerator<int> GetEnumerator
{
	for (int i = 0; i < n; i++) yield return i;
}
```

## The `KeyValuePair<K,V>` Struct type
Holds a key-value pair (known as an *entry*) from a dictionary.
