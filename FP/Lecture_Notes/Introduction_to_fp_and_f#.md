# (Lecture notes) Introduction to Functional Programming and F#
> January 31st 2017

### Side-effects
In functional programming, the model of computation is the application of functions to arguments ==> no arguments.

### Course focus
The focus of the course is on the functional paradigm rather than the language F# itself.

### Advantages of immutability
There are many, but an example is parallel programming where immutability allows you to not care about race conditions and stuff like that.

### Learning outcomes
I must be able to:
- Apply and reflect on theories for modeling, analyzing and constructing functional declarative programs.
- ... concepts behind functional programming compared to imperative and OO programming.
- Construct programs in F# and explain basic principles of functional programming using F#.
- Describe and explain solutions to problems in the context of functional programming.
- Apply core concepts of functional programming.
- Reason about the complexity of functional programs.

### Functions are first-class citizens.
Delegate functions you can pass from place to place. Callbacks, etc. A function as a "first class citizen".

### Strongly typed functional languages
...Like not explicitly writing types and instead relying on the type inferring system to explode if something goes wrong.

### Recursion
Is all over the place. Where you'd use a loop, you'll use recursion (at least in this course).

### Strict vs lazy evaluation
If evaluation is strict, everything is evaluated. In lazy evaluation, you only compute what you need. Benefits of strict evaluation are more compiler warnings whereas benefits of lazy evaluation are better performance and, in some cases, less development pain.

### OCAML vs F#
They are very similar (*"F# is OCAML"*). F# has access to the .NET Core library.

## Imperative model
Describes *how* a solution is obtained. A "plan" for execution.

## Declarative model
Is focused on *what* a solution is.

In functional programming, a program is expressed as a mathematical function: *f : A -> B*.

### State
We are not mutating state, we are manipulating data.

### Lambda-calculus
Is at the core of functional programming.

### Expressions
Everything is expressions. There is not the concept of expressions *and* declarations.

### Tail-recursive calls
Does not require the stack of recursive calls to be maintained (and thus is much more efficient).

### `if-then-else` expressions
Form: if *b* then *e<sub>1</sub>* else *e<sub>2</sub>*.

Pattern matching is generally preferred in practice.

### Booleans
...Are short-circuit evaluated.

### Precedence
`&&` has higher precedence than `||`.

## Lists
Denoted (and instanced): `[]`
A list is a `;`-separated sequence of values **of the same type**:
```fsharp
let list = [2;3;6]
```

### Type variables (`'a'`)
Used when we don't know yet what the type of something is. What you'd typically call generics. The polymorphism principle in functional programming is based on this rule.

### List head and List tail
A list has a head and a tail. For a list *[x<sub>1</sub>, x<sub>2</sub>, ..., x<sub>n</sub>]* where *n >= 1*, the head is *x1* and the tail is *[x<sub>2</sub>, ..., x<sub>n</sub>]*.

### Non-empty lists
Denoted: `x::xs`.

## Infix functions
Infix functions are functions that can be placed in between arguments.
For example, `+` is actually an infix function.

To use an infix function as a regular function, you can wrap it in brackets.
For example:
```fsharp
2 + 3;;

// is the same as

(+) 2 3
```

### Comparisons and functions
You can't compare functions (>=, =, <=, <, >).
