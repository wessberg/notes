# Finite Trees

> HR Chapter 6

## Trees

Trees are structures that may contain subcomponents of the same type.

A list is an example of a tree.

### Recursive types

Th represent a set of values which are trees, we use a *recursive type* in F#.

A *Chinese Box* could have the recursive type definition:

```fsharp
type Cbox = | Nothing
						| Cube of float * Color * Cbox
```

### Tree patterns

Trees can be used in patterns just like tagged values:

```fsharp
match stuff with
 | foo -> Cube(0.5, Red, Nothing)
```

### Expression trees

Can be used to divide an expression up into the symbols or tokens it contains as a representation of the expression.

#### Declaring expression trees

One example could be:

```fsharp
type Fexpr = | Const of float
						 | X
						 | Add of Fexpr * Fexpr
						 | Sub of Fexpr * Fexpr
						 | Mul of Fexpr * Fexpr
						 | Div of Fexpr * Fexpr
						 | Sin of Fexpr
						 | Cos of Fexpr
						 | Log of Fexpr
						 | Exp of Fexpr
```

#### Coverting expression trees to a string representation

Simple enough:

```fsharp
let rec toString = function
  | Const x -> string
  | X -> "x"
  | Add(fe1, fe2) -> "(" + (toString fe1) + ")" + " + " + (toString fe2) + ")"
	// And so on
```

### Parameterized types

*Parameters* of a type is like generics in imperative languages and is written in angle brackets `<..>`.

For example:

```fsharp
type BinaryTree<'a, 'b> =
	| Leaf of 'a
	| Node of BinaryTree<'a, 'b> * 'b * BinaryTree<'a, 'b>
```

## Mutually recursive types and functions

Mutually recursive types are declared by declaring each type individually but adding `and` in between:

```fsharp
type FileSystem = Element List
and Element = | File of string
							| Dir of string * FileSystem
```

Notice how Element appears in FileSystem and FileSystem appears in Element - hence the mutual recursion nature of the types.

To declare mutually recursive functions, it's the same thing with the `and` keyword:

```fsharp
let rec nameFileSystem = function
	| [] -> []
	| e::es -> (namesElement e) @ (namesFileSystem es)
and namesElement = function
	| File s -> [s]
	| Dir(s,fs) -> s :: (namesFileSystem fs)
```