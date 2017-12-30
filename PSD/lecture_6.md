# Lecture 6

> Programming Language Concepts, Chapter 7,
>
> [Fundamental Concepts](https://learnit.itu.dk/pluginfile.php/194545/course/section/95517/strachey-fundamental-1967.pdf)
>
> [Kernighan and Ritchie sections 5.1 - 5.5](https://learnit.itu.dk/pluginfile.php/194545/course/section/95517/the_c_programming_language_2nd_edition.8943347124.pdf)

## Imperative Languages

In imperative programming languages, the value of a variable may be modified by assignment. The course defines a simple language called *micro-C*, which a simplified flavor of *C* with the following abstract syntax:

```fsharp
type expr =
	| CstI of int
	| Var of string
	| Prim of string * expr *expr

type stmt =
	| Asgn of string * expr
	| If of expr * stmt * stmt
	| Block of stmt list
	| For of string * expr * expr * stmt
	| While of expr * stmt
	| Print of expr
```

### Statements vs Expressions

#### Statements

The purposes of executing a *statement* is **to modify the state of the computation**. For example, by modifying the store, by producting some output, etc.

**In most functional languages, there are no statements**.

#### Expressions

The purpose of evaluating an *expression* is to compute a value. In imperative languages, it *is* possible for expressions to modify the store also, for example with *i++* epressions or something like that (incrementing *i* by one).

## Environment and Store

- An *environment* maps variable names (such as `x`) to store locations (such as `0x34B2`).
- An updatable *store* maps locations (such as `0x34B2`) to values (such as `117`).

## `lvalue` vs `rvalue`

There are two kinds of values in imperative languages.
It has to do with whether or not we read the address of the variable or array element or the value associated with it from the store.

### `lvalue`

When a variable `x` or array element `a[i]` occurs as the target of an assignment statement:

`x = e`

or as the operand of an increment operator:

`x++`

or as the operand of an *address operator*:

`&x`

**Then we use the `lvalue` ("left hand side value") of the variable or array element**.

**The `lvalue` is the *location* (address) of the variable or array element in the store**.

Only expressions that have a location in the store can have an `lvalue`. This is also why this expression makes no sense:

`(8 + 2)++`. What are you trying to increment? Sure, it has the `rvalue` 10, but it does not have an `lvalue`.

An *environment* maps names to `lvalue`s. The *store* then maps `lvalue`s to `rvalue`s.

### `rvalue`

When the variable `x` or array element `a[i]` occurs in an expression such as this:

`x + 7`

Then we use its' `rvalue` (right hand side value). The `rvalue` is the *value* stored at the variable's location in the store.

## Parameter passing

When you call a function with arguments, there are multiple ways to actually perform this so-caleld parameter passing:

- **Call-by-value**: A copy of the argument expression's `rvalue` is made in a new location, and the new location is passed to the procedure/function/method. This also means that updates to the given argument do not affect the original `rvalue`.

- **Call-by-reference**: The `lvalue` (location) of the argument expression is passed to the procedure. Here, updates to the argument within the procedure/function/method *do* affect the actual one.

Call-by-reference is really useful when you want to allow a function to actually update the value on a specific store location.

- **Call-by-value-return**: A copy of the argument expressions `rvalue` is made in a new location, and the new location (`lvalue`) is passed to the procedure (like in call-by-value). When the procedure returns, the current value in that location is copied back to the argument expression (`if it has an lvalue`). Neat!

### Call-by-reference in C

While most imperative languages (like Java) only allow calling by value, in C, you can pass a variable *by reference* just by passing its address, such as `&x`, and making the corresponding formal parameter, such as `xp`, be a pointer. Really nice!

## Integers, Pointers and Arrays in C

### Declaring a variable

Declaring a variable is as follows:

`int i`

This reserves storage for an integer and introduces the name `i` for that storage location. **It is not initialized to any particular value**.

### Declaring a pointer

A *pointer* `p` to an integer can be declared:

`int *p`

This reserves storage for a pointer, and introduces the name `p` for that storage location. **It does not reserve storage for an integer**. The pointer is not initialized to any particular value. It is essentially a store *address*.

The integer pointed to by `p` - If it has any! - may be obtained by *dereferencing* the pointer:

`*p`

A *dereferenced* pointer may be used as an ordinary value (an `rvalue`) as well as the destination of an assignment (an `lvalue`):

```c
i = *p + 2;
*p = 117;
```

If you have an `lvalue` and you want to obtain the address for it (e.g. a pointer), you can prepend it with `&`:

```c
p = &i
```

...Will assign the *address* if `i` to `p`.

**The dereferencing operator (*) and the address operator (&) are inverses, so you can go both ways**.

### Array pointers

Actually, given an array `arr`, `arr[0]` is identical to `*arr`.

The reason is that all data is blocks of bytes, and the address of the array is simply the address of the first element of it.