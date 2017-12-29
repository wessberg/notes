# Lecture 4

> Programming Language Concepts, Chapter 5.1 - 5.4, A.11 - A.12, Chapter 6

## Higher-order functional language

A *higher-order* functional language is just one in which a function may be used as a value.

This means that a variable may be avalue, a function may take a function as an argument, a function may return a function, and so on.

## Higher-Order Functions

A higher-order function is a function that takes a function and/or returns a function as an argument

An example of a higher-order function could be `List.map`, for example, since it receives a function it will invoke with arguments. I know this already, so I'm gonna skip ahead (this is my notes, after all :-))

### Infix pipe operator in FSharp

This is actually a higher-order function in that it applies whatever is on the right-hand side to the argument on the left side:

`x |> f` calls the function `f` with `x` as an argument.

## Eager and Lazy Evaluation

Consider this:

```fsharp
if foo then doA else doB
```

Because the `then` or `else` clause is never evaluated if `foo` doesn't respect to true or false respectively, the evaluation of the terms here is lazy. Another example would be inside short-circuiting logical boolean operations such as `foo || bar` where the right-hand side would never evaluate if `foo` already evaluated to true. Here, too, lazy evaluation is important.

Now, if we evaluated these things eagerly, we would compute the value of both the `then` and `else` clause as well as `bar` in the boolean *OR* operation - which both causes unnecessary work and potential side-effects. Sometimes even infinite recursion.

For example, the following custom conditional statement would evaluate the arguments before entering the function block:

```fsharp
let myif b v1 v2 = if b then v1 else v2
```

In general, though, by far the most things are eagerly evaluated in nearly all languages, but as language implementors, we must make these decisions.

**Haskell has lazy evaluation!** All languages that are lazy are purely functional since it is nearly impossible to understand side-effects in combination with the hard-to-predict evaluation order of a lazy language.

This means that invoking a function with an argument is more of a way to have that function "subscribe" to that argument and evaluate it whenever the value associated with the argument changes. Sort of like data-binding, I would say.

## Polymorphic Types

A function is said to be *polymorphic* when it accepts many different types.