# Lecture 1

> Programming Language Concepts, Chapter 1

## Meta Language and Object Language

### Object Language

In linguistics and mathematics, an *object language* is a language we study (such as C++ or Latin).

### Meta Language

A *meta language* is the language in which we conduct or discussions (such as Danish or English).

### Language in the course

In the course, F# will be the meta language of choice.

## Integers

Integers are represented by the constructor `CstI`.

## Operator applications

Operator applications are represented by the constructor `Prim`.

This would be a `BinaryExpression` in Typescript. It represents an operation between two `expr`, for example: `Prim("_", CstI 3, CstI 4)` represents `3 - 4`.

## Expressions

Expressions are represented of an F# datatype `expr`.

```fsharp
type expr =
	| CstI of int
	| Prim of string * expr * expr
```

An `expr` is an *abstract syntax tree* (AST) that represents an expression.

### Examples of expressions

|  Expression  |          Representation in type `expr`          |
|--------------|-------------------------------------------------|
|    `17`      |                    `CstI 17`                    |
|   `3 - 4`    |           `Prim("_", CstI 3, CstI 4)`           |
| `7 * 9 + 10` | `Prim("+", Prim("*", CstI 7, CstI 9), CstI 10)` |

## Evaluating expressions

We can write a function that takes an expression and returns a result. With the simple `expr` type we're working with, a result will always be an integer, so we could write a function of type `eval : expr -> int`:

```fsharp
let rec eval (e: expr) : int =
	match e with
	| CstI i -> i
	| Prim("+", e1, e2) -> eval e1 + eval e2
	| Prim("*", e1, e2) -> eval e1 * eval e2
	| Prim("-", e1, e2) -> eval e1 - eval e2
	| Prim _            -> failwith "unknown primitive";;
```

The *eval* function is an *interpreter* for programs in the expression language.

## Expressions with Variables

We can extend the `expr` type with variables:

```fsharp
type expr =
	| CstI of int
	| Var of string
	| Prim of string * expr * expr
```

And add some new examples:

|  Expression  |          Representation in type `expr`          |
|--------------|-------------------------------------------------|
|    `17`      |                    `CstI 17`                    |
|     `x`      |                    `Var "x"`                    |
|   `3 + a`    |           `Prim("+", CstI 3, Var "a")`          |
| `b * 9 + a`  | `Prim("+", Prim("*", Var "b", CstI 9), Var "a")`|

Now with variables, our program becomes stateful. The `eval` function must also receive an *environment* which associates a value with a variable - so that we know at any given time which value a variable holds.

Such an environment could be:

```fsharp
// 3 is assigned to a, 89 to c, 666 to baf and 111 to b
let env = [("a", 3); ("c", 89); ("baf", 666); ("b", 111)];;
```

The type of an environment with this data type is `(string * int) list`.

A `lookup` function could be:

```fsharp
let rec lookup env x =
	match env with
	| []        -> failwith (x + " not found")
	| (k, v)::r -> if x = k then v else lookup r x;;
```

Which essentially just returns the associated value for a given key.

The `eval` function can then be extended to:

```fsharp
type Env = (string * int) list;
let rec eval expr (env : Env) : int =
	match expr with
	| CstI i             -> i
	| Var x              -> lookup env x
	| Prim("+", e1, e2)  -> eval e1 env + eval e2 env
	| Prim("*", e1, e2)  -> eval e1 env * eval e2 env
	| Prim("-", e1, e2)  -> eval e1 env - eval e2 env
	| Prim _             -> failwith "unknown primitive";;
```

The `env` may contain multiple identical keys. The `lookup` function returns the first match. The order of identical keys could be used to say something about the scope, e.g. which assigned value to `x` is has highest priority (for example, if `x` is overwritten inside the current block scope).

## Syntax and Semantics

### Syntax

*Syntax* deals with form: Is this program text well-formed?

There are two kinds of syntax, *concrete* and *abstract*.

#### Concrete syntax

This is a representation of a program as text, with whitespace, parentheses, curly braces, etc.

#### Abstract syntax

This is a representation of a program as a tree.
Here, stuf like whitespace, parentheses, etc have been abstracted away.

### Semantics

*Semantics* deals with meaning: What does this (well-formed) program mean? How does it behave? What happens when we execute it?

There are two kinds of semantics: *dynamic semantics* and *static semantics*.

#### Dynamic semantics

These concerns the meaning or effect of a program at run-time. What happens when it is executed? Dynamic semantics may be expressed by `eval` functions.

#### Static semantics

These concerns the compile-time correctness of the program. Are variables declared, is the program well-typed and so on. These are all things that can be checked without actually executing the program.

### How syntax and semantics are described in the course

- Abstract syntax are described using F# datatypes.
- Concrete syntax are described using lexer and parser specifications.
- Semantics (both static and dynamic) are described with F# functions.