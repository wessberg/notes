# Indentation, Verbosity, Records, Tagged values and lists

> F# syntax: [indentation and verbosity by Scott Wlaschin](http://fsharpforfunandprofit.com/posts/fsharp-syntax/)
>
> HR, Section 3.8 - 3.11
>
> HR, Chapter 4

## Tagged values

Tagged values are used when grouping together values of different kinds to form a single set of values.

In F#, a collection of tagged values is declared by a *type* declaration:

```fsharp
type Shape = | Circle of float
						 | Square of float
						 | Triangle of float*float*float
```

I like to see it as a type *alias* for something else. Another name for a type.

What makes them special, though, is that they have constructors:

### Tagged value constructors

When you have a tagged value, they suddenly have value constructors. This means that in the above example, you could do:

```fsharp
Circle 1.2
val it : Shape = Circle 1. 2
```

A constructor is just a function.

### Equality in tagged values

Two tagged values are equal if they have the same constructor and their components are equal:

```fsharp
Circle 1.2 = Circle(1.0 + 0.2) // true
Circle 1.2 = Square 1.2 // false
```

## Enumeration types (enums)

An `enum` is a special kind of tagged value without any constructor arguments:

```fsharp
type Color = Red | Blue | Green | Yellow | Purple
type Color =
	| Red
	| Blue
	| Green
	| Yellow
	| Purple

Green
val it : Color = Green
```

And you can pattern match on them:

```fsharp
let niceColor = function
	| Red -> true
	| Blue -> true
	| _ -> false
```

## Catching exceptions with `try ... with` expressions

You can catch exceptions:

```fsharp
let foo =
	try
		something
	with
		| Failure s -> s
```

where the thing inside the `with` is like a `catch` clause in other languages except the `with` part must be a match expression.

## Partial functions - The `Option` type

The `Option` type is F#'s way of handling conditional/nullable values.

With it, `None` is used as result for arguments where the function is undefined while `Some v` is used when the function has value *v*.

This way, you can check for undefined values, especially useful in match functions.

## Lists

Let's start with a definition. A *list* in F# is a finite sequence of values **of the same type**.

### Head

The first element of a list is called its *head*.

### Tail

The rest of the list, anything but the head, is called the *tail*.

### Declaring lists

To declare a list, write the values separated by semi-colons:

```fsharp
let myList = [2; 3; 2;]
```

### Equality of lists

Two lists are equal when all of their elements are equal - in order.

So,:

```fsharp
[2;3;3;] = [2;3;3;] // false!
[2;3;3;] = [2;3;3;] // true!
```

Lists of functions can not be compared since functions cannot be compared in F#.

## The `cons` operator (`::`)

The *cons* operator (`::`) is an infi operator that builds a list from its head and its tail:

```fsharp
let x  = 2::[3;4,5;]
val x : int list = [2; 3; 4; 5]

let y = ""::[]
val y : string list = [""]
```

As you can see, the left-hand token is "piped" into the list to the right of the cons operator.

In other words, the operator *associates* to the right.

## Patterns for matching on lists

- `[]` matches the empty list.
- `[x]` matches the list of 1 item.
- `x::xs` matches a non-empty list.

### Deconstructing a list

To break list elements up into two variables - the head element and the remaining list:

```fsharp
let x::xs = [1;2;3]
val xs : int list  = [2; 3]
val x : int = 1
```

This generalizes to as many elements as you want. For example:

```fsharp
val x0::x1::xs = [1; 2; 3; 4;]
val xs : int list = [3; 4;]
val x0 : int = 1
val x1 : int = 2
```

Otherwise you can do deconstructing as you know and love already:

```fsharp
let [foo; bar;] = [1; 2]
val foo : int = 1
val bar : int = 2
```

### Range expressions

You can declare a list from a range if the identifiers used are number types:

```fsharp
[b .. e]
// or
[b .. s .. e]
```

It will populate the lists with the range of numbers in between.

The expression *s* in the middle is called the **step**. More on that later.

For example:

```fsharp
[ -3 .. 5]
val it : int list = [-3; -2; -1; 0; 1; 2; 3; 4; 5;]
```

Very convenient!

#### The *step*

The thing in the middle of a range list constructor (for example, in `[b .. s .. e]`, *s* is the step). The generated list will be either ascending or descending depending on the sign of *s*:

```fsharp
[6 .. -1 .. 2]
```

Generates a list of integers from 6 to 2.

## Typical recursions over lists

### Summing a list of integers

```fsharp
let rec sum1 = function
	| [] -> 0
	| x::xs -> x + sum1 xs
```

### Layered patterns

A layered pattern has the form `pattern *as* something`.

It simples binds a pattern to an identifying name.

You can do:

```fsharp
let rec succPairs = function
| x0::(x1::_ as xs) -> (x0,x1) :: succPairs cs
| _ -> []
```

### Checking if something is a member of a list

```fsharp
let rec isMember x = function
	| y::ys -> x=y || (isMember x ys)
	| [] -> false
```

### Concatinating lists with `@`

You can join two lists **of the same type** with `@`:

```fsharp
[x1; x2] @ [y0; y1] = [x0; x1; y0; y1]
```

### Reversing a list

This is done with `List.rev`.