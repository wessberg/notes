# Modules

> HR Chapter 7

Modules are declared in *signature* and *implementation* files, like in C.

## Why modules

The benefits of modules are obvious to me. I won't go into detail here.

## Implementation file

This one contains the declaration of the entities in the library.
It is a *.fs* file.

## Signature file

This one specifies **the user's interface to the library**.
It is a *.fsi* file.

## Libraries (.dll) files

When the *.fs* and *.fsi* files are compiled, together they form a *library*.
In practice, this means that a *.dll* file is generated, which is the Binary output file from the F# compiler.

## Requirements

The implementation **must** match the signature from the *.fsi* file. You can declare local stuff inside an implementation file, but only the types and functions that are declared in the signature file will be accessible from the outside.

## Hidden types

Hidden types are types declared in an interface file without a type expression.

For example:

```fsharp
type Vector
```

...Is a hidden type.

We can then declare some publicly accessible functions that does operations on the type or returns "instances" of it. For example:

```fsharp
module Vector
type Vector
val ( +.) : Vector -> Vector -> Vector
val make : float * float -> Vector
// ... and so on
```

This would make the infix function `+.` available and the `make` function available for constructing new `Vector`s.

The corresponding *implementation* file would then actually implement these things and must hold a definition of the hidden type:

```fsharp
module Vector
type Vector = V of float * float
let (+.) (V(x1,y1)) (V(x2,y2)) = V(x1+x2, y1+y2)
let make (xy) = V(xy)
```

Notice how `val` is used to declare something while `let` is used to implement it.

Notice also the `V` identifier in the type declaration. That's because hidden types must be tagged types or record types, so we have to add a tag. In this case, we used `V`, but we could have used anything.

## Using a library

Simply add an `open <library-name>` statement in top of the files in which you want to use the module.

For example:

```fsharp
open Vector
let a = make(1.0, -2.0)
// ...and so on
```

The *structure* of the type is hidden to the consumer of the library. Instead, only the type *name* will appear, for example in F# Interactive.

## Type augmentation

You can add declarations to the definition of a tagged type or a record type. It allows declaration of overloaded operators.

It looks pretty Object-Oriented.

Here's an example of a signature file:

```fsharp
module Vector
[<Sealed>]
type Vector =
	static member ( +) : Vector * Vector -> Vector
val make : float * float -> Vector
// ... and so on
```

The cool thing is that you can "override" the behavior for stuff like adding two custom entities together, such as Vectors.

Here's the corresponding implementation:

```fsharp
module Vector
type Vector =
	| V of float * float
	static member (+) (V(x1,y1), V(x2, y2)) = V(x1+x2,y1+y2)
let make(x,y) = V(x,y)
```

The reason why OO-like features are suddenly introduced is because they are the only way to obtain the effect of overriden operators.

## The `[<Sealed>]` attribute

It is **mandatory** when a type augmentation is used. Sealed classes cannot be subclassed.

## Type Extension

Type Extension is another way to do the same thing as before.
In this case, rather than declaring all the members at the same time of the type declaration, we can use the syntax: `type <typename> with ...<members>`:

```fsharp
module Vector
type Vector = V of float * float
let make(x,y) = V(x,y)
type Vector with
	static member (+) (V(x1,y1), V(x2,y2)) = V(x1+x2, y1+y2)
```

## Classes and objects

A *class definition* looks like an augmented type definition where the type expression has been removed and replaced by declarations of *constructor functions*:

```fsharp
type ObjVector(X: float, Y: float) =
	member v.x = X
	member v.y = Y
	static member (+) (v1: ObjVector, v2: ObjVector) = ObjVector(v1.x + v2.x, v1+y + v2.y)
	// and so on
```

In this example, `(X: float, Y: float` is the constructor arguments.