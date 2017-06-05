# Imperative features

> HR Chapter 8

Imperative features are the root of all evil according to the mind of the purest Haskell-fetichist programmer, but F# has lots of OO features under the hood due to the fact that it is based on the .NET runtime and is fully compatible with the bundled class libraries of it - and for some weird reason, the course wants to spend precious time delving into these features of F#.

So here goes.

## Locations (and the `mutable` keyword)

Locations are what's in the end of a pointer: a specific part of computer memory where the F# program may store different values at different points in time.

To obtain a location, we use the `mutable` keyword:

```fsharp
let mutable x = 1
val mutable x : int = 1
```

We can now assign new values to `x` like we would in imperative languages.

## Mutable record fields

Individual fields of records may be mutable:

```fsharp
type intRec = { mutable count : int}
```

## While loops

Syntax: `while <condition> do <block>`.

## Arrays

Here's how to declare an array:

```fsharp
let a = [|4;5,6;7|]
val a : int [] = [|4; 5; 6; 7|]
```

Notice the pipes. These are what makes an array different from a simple list.
The type of an array is like you know it from other imperative languages: `<type>[]`.

### Advantages of arrays

Any array location can be accessed and modified in constant time. But at the same (and for the same reason), it is mutable and thus leads to all the potential drawbacks of mutability.

Also, an arrya cannot be extended by more elements in a simple way since the adjacent physical memory after the last element in the array might be occupied for other use.

#### Accessing array elements

Like lists:

```fsharp
let b = [|1; 2; 3;]
b.[2] // The item on the 2nd index position of the array.
```