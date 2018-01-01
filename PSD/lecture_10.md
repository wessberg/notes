# Lecture 10

> Programming Language Concepts, Chapter 11

## A Recursive but not tail-recursive function

A recursive function is one that may call itself (duh).

**A function call is a *tail call* if it is the last action of the calling function**.

This also means that inside a recursive function `facr`, a call such as this:

`n * facr(n-1)` is **not** tail-recursive since the last action of the function isn't to call itself recursively but instead to apply the result of calling itself recursively with multiplying with `n`.

## Continuations

A *continuation* is an explicit representation of "the rest of the computation", e.g. what will happen next.

This is usually implicit in a program: after executing one statement, the computation will continue with the next statement.

When returing from a method, the computation will continue where the method was called.

Making the continuation explicit has the advantage that we can ignore it and that we can have more than one.

A continuation is typically expressed in the form of a function *from* the value of the current expression *to* the result of the entire computation.

### Continuation-passing style (CPS)

This is a programming style where a function takes an extra argument, the continuation `k` which decides what will happen to the result of the function.

Basically, a callback.

## Writing a interpreter in CPS

When an interpreter for a functional language is written in continuation-passing style, a continuation is a function from the value of an expression to the "answer" or "final result" of the entire computation of the interpreted program.

When an interpreter for an imperative language is writtein in continuation-passing style, a continuation is a function rom a store to the "answer" (the "final store") produced by the entire computation.

### Why CPS is smart

The big advantage is that by making "the rest of the computation" explicit as a continuation parameter, the interpreter is free to ignore the continuation and return a different kind of "answer".