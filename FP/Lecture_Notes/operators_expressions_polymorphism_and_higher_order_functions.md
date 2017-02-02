# (Lecture notes) Operators, Expressions, Polymorphism and higher-order functions in F#
> February 2nd 2017

### Closure
Represents a function as a value and whatever scope (environment) that it contains. It doesn't contain the actual values but rather pointers to the specific memory locations (i.e. it doesn't copy values, just a representation of the memory).

### The `**` operator
Is a sugar-coated `Pow` function. It raises the left-hand value to the power of the right-hand value.

### `match` expressions
Pretty cool:
```fsharp
match e with
	| path1 -> e1
	| pathn -> en
```

And, as written in the primary notes, these can be used in higher-order functions. That's, to me, what makes them really awesome.
It is especially useful when given a tuple and you can be specific about which part of the tuple to match with, e.g.
```fsharp
let rec power (x, n) = match n with
                       | 0 -> 1.0
											 | n' -> x * power(x, n' - 1)
```

### The `<<` operator
Can be used in between two functions to compose them.
```fsharp
let h = f << g    // h represents the combined function of f and g.
```

This means that the `f` must return some value that we can pipe on to `g` which then must return some value that we can pipe to a combined function of the two.

### Equality
Remember, equality checks are done on values, not on any referential concept like you are so used to. For example, two tuples are equal if all of their contents are the same and they have the same length. Similarly, two records are identical if they contain the same labels and values associated with them.

#### Equality checks on `float`s works
In F#, two floats are equal if their base contents are the same, e.g. 5.0 and 5.0. In other languages with 64-bit floats, they might contain so many decimals that their contents are practically never equal.

### The `when` keyword
```fsharp
let f  =
	match x with
	(a, b) when a = b -> a
	| (a, b) when a < b -> b
	| (a, b) when a > b -> b
```

The `when` keyword can be used in place of *if-then-else* patterns and is especially useful in pattern matching.
**Disclaimer: It appears to be a purely F# related feature. OCAML and Standard ML doesn't support it. So don't get too used to it**.

### Quick words on indentation
Unless you are declaring the body for a function (in which case the body should be indented), expressions that are in the same scope should be placed on the same indentation level.
