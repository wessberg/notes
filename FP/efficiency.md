# Efficiency

> HR Chapter 9

Has to do with writing efficient F# code, some stuff about garbage collection, tail recursion, heaps and the stack.

## Memory management

The memory used by an F# program is split into a stack and a heap:

- *Primitive values* (such as numbers, booleans, etc) are allocated on the stack.
- *Composite values* such as lists, trees, closures and most objects are allocated on the heap.

The heap is the thing the garbage collector has a lot of fun doing community service inside of.

## Tail Recursion

Tail recursion is a form of recursion that doesn't take up additional stack space.

If the last thing to happen in a function is the recursive call, there is nothing else to execute after the result of the recursive call comes back, and thus the runtime doesn't need to keep the caller on the stack.

A recursive function *f* can be transformed into a tail-recursive one.

Consider this:

```fsharp
let rec recursiveFunction x =
	if (x <= 1) then foo else x * recursiveFunction
```

Here, whatever the result of the recursive call is need to be multiplied by `x` and thus the stack needs to be persisted so the runtime can perform that computation whenever the result comes back. So this is **not** a tail recursive function.

*This* is a tail recursive function:

```fsharp
let factorial x =
    // Keep track of both x and an accumulator value (acc)
    let rec tailRecursiveFactorial x acc =
        if x <= 1 then
            acc
        else
            tailRecursiveFactorial (x - 1) (acc * x)
    tailRecursiveFactorial x 1
```

Since the last thing to happen in this function is to return the result of the recursive call, this *is* a tail recursive function.

Not only does this eliminate stack overflows (stack space are typically <=1mb), but it also brings performance optimizations (~30%).