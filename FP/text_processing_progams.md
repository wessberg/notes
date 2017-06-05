# Text processing programs

> HR Chapter 10 and 11
>
> [Active patterns](https://docs.microsoft.com/en-us/dotnet/fsharp/language-reference/active-patterns)

## Text processing (and Regular Expressions)

The things about text processing was primary about regular expressions, which I do not feel the need to write any further notes on. Then there was a bunch of examples of parsing XML and HTML but overall it was a TL;DR feeling.

## Sequences

A *sequence* is a **possibly infinite** ordered collection of elements.

### The `seq<'a>` type

The `seq<'a>` type is an F# alias for the .NET type `IEnumerable<'a>`.

It can be formed like a list and a list is just a specialization of the concept after all. This means you can do:

```fsharp
seq [10; 7; -25]
val it : seq<int> = [10; 7; -25]
```

### Infinite sequences

These are the more interesting parts of it.
It is a sequence that is potentially infinite but where you perform operations on a finite subset of the sequence at every given moment in time.

There is also a lot of stuff about F# with LINQ and other .NET specifics. I skipped that.