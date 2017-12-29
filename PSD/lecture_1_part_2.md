# Lecture 1 Part 2

> Programming Language Concepts, Chapter 2

## Interpreter

An *Interpreter* executes a program on some input, producing an output or result.

An interpreter is usually a program, but a CPU is an interpreter as well.

## Interpreted Language L vs Implementation Language I

- The *Interpreted Language L* is the language of the programs being excecuted.
  - If this language is pretty low-level (e.g. a sequence of simple instructions and looks like machine code), the interpreter is often called an abstract machine or virtual machine.

- The *Implementation language I* is the language in which the interpreter is written.

## Compiler

A *compiler* takes as input a source program and generates as output another program **called a target program**. This one can be executed.

There are three languages to distinguish:

- The source language *S* (of the input program(s))
- The target language *T* (of the output program(s))
- The implementation language *I* (that the compiler is written in)

The compiler doesn't execute the program. Instead, it generates a target program from the input program.

It is the interpreter which can then execute the target program(s).

## Scope and Bound and Free Variables

### Scope

The *scope* of a variable binding is that part of a program in which it is visible.

For example:

```fsharp
let f x = x + 3
```

Here, the scope of `x` is just the function body, `x + 3`.

#### Static scope

If the scopes of bindings follow the syntactic structure of a program, a language has *static scope*. This goes for by far the most languages.

#### Nested scope

If an inner scope may create a "hole" in an outer scope by declaring a new variable with the same name, where the second binding shadows the first one, the language has a nested scope.

For example:

```fsharp
let f x = (let x = 8 in x * 2) + (x + 3)
```

 Another example is if a parameter or local variable in a method shadows a field from an enclosing class.

This also goes for by far the most languages.

### Bound and free occurrences of a variable

A variable occurrence is *bound* if it occurs within the scope of a binding for that varible. Otherwise, it is *free*.

So, here, *x* occurs bound in the body of the let-binding:

```fsharp
let x = 6 in x + 3
```

And here, *x* occurs free:

```fsharp
let y = 6 in x + 3
```

## Expressions with Let-Bindings and Static Scope

The `expr` type can be extended with let-bindings of the form `let x = e1 in e2 end`:

```fsharp
type expr =
	| CstI of int
	| Var of string
	| Let of string * expr * expr
	| Prim of string * expr * expr
```

We can extend the `eval` function:

```fsharp
let rec eval e (env : Env) : int =
	match e with
	| CstI i               -> i
	| Var x                -> lookup env x
	| Let (x, erhs, ebody) ->
		let xval = eval erhs env
		let env1 = (x, xval) :: env
		eval ebody env1
```

Notice how we evaluate the right-hand side (`erhs`) in the same environment as the entire let-expression and obtain a value, `xval`, for `x`. Then we create a new environment, `env1`, by adding the association `(x, xval)` and interpret the let-body `ebody` in that environment. Finally we return the result as the result of the let-binding.

The new binding for `x` will shadow any existing binding of `x`.

The new environment, `env1` is temporary and only lives for as long as the local let-binding of `x` is alive (e.g. is still referenced by something).

## Closed Expressions

An expression is *closed* if no variable occurs free in the expression. To test whether an expression is closed, the following function can be used:

```fsharp
let rec closedin (e: expr) (vs: string list) : bool =
	match e with
	| CstI i               -> true
	| Var x                -> List.exists (fun y -> x = y) vs
	| Let (x, erhs, ebody) ->
		let vs1 = x :: vs
		closedin erhs vs && closedin ebody vs1
	| Prim(ope, e1, e2)    -> closedin e1 vs && closedin e2 vs
```

This function checks of an expression `e` is being closed in a list, `vs` of bound variables.

A constant (`CstI`) is always closed. **A variable occurrence `x` is closed in `vs` if `x` appears in `vs`**.

## Substitution: Replacing Variables by Expressions

We can have an environment, `env`, that maps some variable names to expressions. We can then perform *substitution* by replacing free variables by expressions.

So, if the environment says that variable `z` should be replaced by the expression `5-4`, it would be:

```fsharp
[("z", Prim("-", CstI 5, CstI 4))]
```

## Integer addresses instead of names

For efficiency, symbolic variable names are replaced by variable addresses (integers) in real machine code and in most interpreters.

One way to do so is to replace symbolic variable names by numeric variable indexes.

## Stack Machines for Expression Evaluation

*Stack machines* are often the whones to evaluate programs.
A Stack machine is an Interpreter that implements an Abstract Machine.

Stack machine instructions for a very simple example language without variables could be expressed:

```fsharp
type StackMachineInstruction =
	| CstI of int
	| Add
	| Sub
	| Mul
	| Dup
	| Swap
```

### State of a Stack Machine

The state is a pair `(c, s)`.

- `c`, the *control*, is the sequence of instructions yet to be evaluated.
- `s`, the stack, is the list of values, namely, intermediate results.

The top of the stack is to the right.

So, if the two top-most stack elements are 5 and 7, the stack has the form: `s, 7, 5` (because 5 is highest on the stack).

#### Termination of a Stack Machine (and results)

The machine terminates when there are no more instructions to terminate.

The result of a computation is the value, `v`, on top of the stack when the machine stops!