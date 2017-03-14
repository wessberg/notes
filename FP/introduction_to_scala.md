# Introduction to Scala
> [A Scala tutorial for Java programmers (2011)](https://learnit.itu.dk/pluginfile.php/166124/course/section/88010/ScalaTutorial.pdf)
>
> [An overview of the Scala programming language (2006)](https://learnit.itu.dk/pluginfile.php/166124/course/section/88010/ScalaOverview.pdf)
>
> [Scala homepage](http://www.scala-lang.org/)
>
> [scala.zip](https://learnit.itu.dk/pluginfile.php/166124/course/section/88010/scala.zip)

Scala is designed to
- Work with the Java platform
- Be somewhat easy to pick up if you know Java

It is based on Java's virtual machine.

### Code example:
```scala
object PrintOptions {
	def main(args: Array[String]) = {
		for (arg <- args; if arg startsWith "-")
			println(arg substring 1)
	}
}
```

### Compiling and running scala
We use `scalac` to compile `*.scala` files.

We use `scala` to run the object class file.

The `java` runtime is used with Scala's libraries.

### Syntax
- All declarations start with a keyword (so, no: `int x`).
- `Unit`, () and {} can often be left out.
- **All values are objects and have methods**.
	-	So, you can do `2.to(19)` for example, even though it looks like a primitive value.
- All operators are methods
	-	So, `x+y` are the same as `x.+(y)`

- The base type of all types is: `Any`. For object types, there is a `AnyRef` type that extends `Any` and that all object types derive from, and for primitive types there is a `AnyVal` type that extends `Any` and that all primitive types derive from.

## Object-oriented programming in Scala
### Singletons (object declaration)

- Scala has no static fields and methods
- An `object` is a singleton instance of a class:
```scala
object PrintOptions {
	def main(args: Array[String]) = {...}
}
```

Can create an app as a singleton App:
```scala
object ListForSum extends App {
	val xs = List(2,3,5,7,11,13)
	var sum = 0
}
```

### Classes
Like so much else:
```scala
abstract class Person (val name: String) {
	def print()
}

class Student (override val name: String, val programme: String) extends Person(name) {
	def print() {
		println(name + " studies" + programme)
	}
}
```

### Anonymous subclass and instance:
```scala
val s = new Student("Kasper", "SDT") {
	override def print() {
		super.print()
		println("and something else")
	}
}
```

### Traits
A fragment of a class. Is like a mixin.
```scala
trait Counter {
	private var count = 0
	def increment() { count += 1}
	def getCount = count
}
```

Allows mixin og any number of traits:
```scala
class CountingPerson (override val name: String) extends Person(name) with Counter
```

### Polymorphism (Generics)
written: `Type[GenericTypeName]`

For example:
```scala
trait Ordered[A] extends java.lang.Comparable[A] {
	def compareTo (that: A): Int
}
```

### Lists
Written: `List(1,2,3)` or `1 :: 2 :: 3 :: Nil` where `Nil` is the empty list.
**You can have lists of non-uniform types in Scala**.

## Functional programming in Scala

### Printing the elements of a list (four ways)

1.
```scala
for (x <- xs)
	println(x)
```

2.
```scala
xs foreach { x => println(x)}
```

3.
```scala
xs.foreach(println)
```

4.
```scala
xs foreach println
```

### Anonymous functions (three ways):
1.
```scala
var sum = 0
for (x <- xs)
	sum += x
```

2.
```scala
var sum = 0
xs foreach { x => sum += x }
```

3.
```scala
xs foreach { sum += _}
```

In the third example, the `_` refers to an anonymous argument.

### List functions, pattern matching

Compute the sum of a list of integers
```scala
def sum(xs: List[Int]): Int =
	xs match {
		case Nil => 0
		case x::xr => x + sum(xr)
	}
```

A generic version
```scala
def repeat[T] (x: T, n: Int): List[T] =
	if (n == 0)
		Nil
	else
		x :: repeat(x, n-1)
```

Using fold:
```scala
def sum1 (xs: List[Int]) =
	xs.foldLeft(0)((res,x) => res + x)
```

Or using fold, but more compact (with anonymous parameter names):
```scala
def sum2(xs: List[Int]) =
	xs.foldLeft(0) (_+_)
```

A `foreach` function from `trait` `Traversable[T]`:
```scala
def foreach[T] (xs: List[T], act: T => Unit): Unit =
	xs match {
		case Nil => {}
		case x::xr => { act(x); foreach(xr, act) }
	}
```

### Case Classes
- They do not require the `new` keyword.
- They only have public fields
- We can use them for equality.


Written, for example:
```scala
case class CstI(value: Int) extends Expr
```

### Type members in Classes
Type members are abstract types that are members of a class.
We can use them to further restrict/constraint the types we expect, for example in method arguments:

```scala
class Food

abstract class Animal {
	type SuitableFood <: Food
	def eat(food: SuitableFood)
}
```
