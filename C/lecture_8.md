# Lecture 8

> CS:APP, Chapter 5.1 - 5.3

There is always a trade-off between writing super optimized, efficient code and writing elegant, compact code.

The compiler can do lots of things to help, but some things it just cannot rewrite. Best practice is to optimize the source code until the compiler can take over from there and do the rest. Essentially, it is about avoiding bottlenecks that things like `gcc` would'nt be able to optimize.

Even though the optimization compilers are great, still it is very possible to write C code that, when compiled with the lowest optimization options, is still much faster than another, more naive C implementation, compiled with the highest optimization settings.

## Memory aliasing

This is the case where two pointers designate the same memory location. When the compiler is optimizing, it must assume that different pointers may be aliased.

This is one of the major *optimization blockers*. It severly limits the opportunities for a compiler to generate optimized code.

## Expressing Program Performance

### Cycles Per Element (CPE)

*Cycles per element* (CPE) is a way to express program performance in a way that can guide us in improving the code.

#### Processor speed

The sequencing of activities by a processor is controlled by a clock providing a regular signal of some frequency, usually expressed in *gigahertz* (GHz), billions of cycles per second.

So, when a system is said to have a "4 GHz" processor, it means that the processor clock runs at *4.0 * 10<sup>9</sup>* cycles per second.

The *period* of a 4 GHz clock can be expressed as either 0.25 nanoseconds or 250 picoseconds.

For a programmer, it is better to express measurements in clock cycles rather than nanoseconds or picoseconds.

We care about how *many* instructions are being executed rather than how fast the clock runs!

### Hidden asymptotic inefficencies

Some seemingly trivial pieces of code may have quadratic execution times!

For instance, if we take the length of a long string inside a loop or otherwise work with a string (lowercasing it, uppercasing it, etc). Whatever we can take out of a loop (so that it will execute once, before the loop body) will help enormously.

## Meta-programming

This is the design of programs whose input and output are programs themselves!

We use it for things like improving performance (for example, loop unrolling).

### Using `#define` for preprocessor macros to optimize at compile time

We can use the `#define` macro to help with these. The C preprocessor replaces whatever comes after `#define` and all references to it in the source code.

This is essentially partial evaluation!

FOr example:

```c
#define SWAP(a, b, type) { type c; c = b; b = a; a = c; }

int main () {
  int a = 3;
  int b = 5;
  SWAP(a, b, int);
  return 0;
}
```

The beauty of it is that the swap happens at compile-time, removing the need for computing it at run-time. And, we get all the nice abstraction benefits of being able to invoke the function. So, in that way, it's a win-win. Our code is easy to read, *and* it is efficient.

## Abstract Data Types

You can do things like:

```c
struct device {
  int (*open)(unsigned mode);
  int (*close)(void);
  // ...
};
```

Which is essentially a `struct` which contains function pointers. This enables invoking members of the struct, essentially allowing object-oriented C programming (so that you can do stuff like `device.close()`).

## POSIX

Means *Portable Operating System Interface*.

The goal is to be a standard library for any Unix system. It contains things like: Core services, File system, Pipes, I/O, Threads, the C library, extensions, etc.

## GNU

Gnu means *Gnu's not Unix*. This is the name for the complete Unix-compatible software system.

## Free Software

In Open-Source, it is free as in "free speech" rather than as in "free beer".

A program can be entitled *free software* if the users os it have the four essential freedoms:

- The freedom to run the program as they wish, for any purpose.
- The freedom to study how the program works, and change it so it functions as they wish. This requires access to the source code (hence, Open Source).
- The freedom to redistribute copies.
- The freedom to distribute copies of their modified versions to others.

## Variadic Functions

These are functions that **take a variable number of parameters**. So, it is essentially functions with ...rest arguments.

One example in C is the `printf` function: `printf("&d: %s", a, b);`.

### Declaring a Variadic function in C

```c
int func (const char* a, int b, ...) {
  // body of the function has been removed
}
```

You use the helper `va_start` to initialize an argument pointer variable of type `va_list`. **This points to the first optional argument**.

Then, following that, each successive call to `va_arg` access the optional arguments in sequence.

You call `va_end` to indicate that you're finished with the argument pointer.