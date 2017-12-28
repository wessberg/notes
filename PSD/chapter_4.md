# PSD - Chapter 4

> Mogensen BCD sections 3.12 - 3.17
> Programming Language Concepts, Chapter 4

## LL parsers

LL parsers are ones that reads the input from left to right and derives from left in a top-down approach.

There are two implementation methods relevant to the course: Recursive descent and a table-based approach.

### Recursive decent

Here, grammar structure is directly translated into the structure of a program.

It uses recursive functions to implement predictive parsing.

**The central idea is that each nonterminal in the grammar is implemented by a function in the program**.

Each such function looks at the next input symbol in order to choose one of the productions for the nonterminal.

The right-hand side of the chosen production is the nused for parsing:

1. A terminal on the right-hand side is matched against the next input symbol.
2. If they match, we move on to the following input symbol and the next symbol on the right-hand side. Otherwise, an error is reported.
3. A nonterminal on the right-hand side is handled by calling the corresponding function and, after this call returns, continuing with the next symbol on the right-hand side.
4. When there are no more symbols on the right-hand side, the function returns.

### Table-driven

Here we encode the selection of productions into a table instead of in the program text.

A simple non-recursive program uses this table and a stack to perform the parsing.

### Conflicts

There is a conflict when a symbol *a* allows several choices of production for nonterminal *N*.

This may be caused by an ambiguous grammar, but sometimes also unambiguous grammars can cause conflicts.

## Rewriting a grammar for LL parsing

If we want to do LL parsing, we should eliminate *left-recursion* and *left-factorisation* from the grammar(s). Not all grammars can be rewritten for LL parsing in which case stronger parsing techniques must be used.

### Eliminating left-recursion

Left-recursive productions produces overlaps in the grammar, as the *FIRST* set of a left-recursive production will include the *FIRST* set of the nonterminal itself and hence be a superset of the *FIRST* sets of all the other productions for that nonterminal.

### Construction of LL parsers summarized

1. Eliminate Ambiguity
2. Eliminate left-recursion
3. Perform left factorisation where required
4. Add an extra start production *S' → S$* to the grammar.
5. Calculate *FIRST* for every production and *FOLLOW* for every nonterminal.
6. For nonterminal *N* and input symbol *c*, choose production *N → α* when:
	- *c ∈ FIRST(α)*, or
	- *Nullable(α)* and *c ∈ FOLLOW(N)*.

### Problems with LL Parsing

The main problem is hat most grammmars need extensive rewriting to get them into a form that allows unique choice of production!

LR parsing to the rescue:

## LR Parsing (SLR parsing)

LR parsing accepts a much larger class of grammars than LL-parsing.

And, stuff like precedences for resolving ambiguity can be declared externally, instead of requiring the grammars themselves to be unambiguous.

*SLR-parsing* is a simple form of LR-parsing.

Means: *Simple Left Right*.

- *Left* indicates that the input is read from the left to the right.
- *Right* means that a rightmost derivation is built.

LR parsers are table-driven bottom-up parsers and has two kinds of actions:

- **shift**: A symbol is read from the input and pushed on to the stack.
- **reduce**: The top *N* elements of the stack hold symbols identical to the *N* symbols on the right-hand side of a specified production. These *N* symbols are by the reduce action **replaced by the nonterminal at the left-hand side of the specified production**.

Therefore, LR Parsers are also called *shift-reduce parsers*.

At every step, the underlying DFA reads the stack from bottom to top, and the action is determined by looking at the DFA state and the next input symbol.

The DFA is actually stored as a table where we cross-index a DFA state with a symbol (which may be a terminal or a nonterminal) and find one of the following actions:

- *shift n*: Read the next input symbol and push state *n* on the stack.
- *go n*: Push state *n* on the stack
- *reduce p*: Reduce with the production numbered *p*
- *accept*: Parsing has completed successfully.
- *error*: A syntax error has been detected

## Using LR-parser generators

### LALR

Most LR-parser generators use an extended version of the SLR construction called *LALR(1)*. *"LA"* is short for *lookahead* and the (1) indicates that the lookahead is one symbol - the next input symbol.

### Declarations and actions

Each nonterminal and terminal is declared and associated with a data-type. This is used to hold the values that are associated with the tokens that come from the lexer, e.g. the values of numbers or names of identifiers.

For each production, an *action* is assigned. The action is a piece of program text that is used to calculate the value of a production that is being reduced.

### Abstract Syntax

Abstract syntax keeps the essence of the structure of the text but omits the irrelevant details. An abstract syntax tree is a tree structure where each node corresponds to one or more nods in the concrete syntax tree.

## Test case: MicroML

MicroML is a small language that Chapter 4 deals with.

Here's its' `expr` type:

```fsharp
type expr =
	| CstI of int
	| CstB of bool
	| Var of string
	| Let of string * expr * expr
	| Prim of string * expr * expr
	| If of expr * expr * expr
	| Letfun of string * string * expr * expr
	| Call of expr * expr
```

### Structural Operational Semantics/Natural Semantics

This is another way to present the evaluation of functions.

They contain *rules*, each of which has a zero or more *premises* (above the line), a horizontal line, and one *conclusion* (below the line).

It says that if all premises hold, then the conclusion holds too.

#### Conclusions

Each conclusion, and most of the premises, consists of a *judgement ρ |- e ⇒ v*.

It says: "within environment *ρ*, evaluation of the expression *e* terminates and produces value *v*.

#### Value environment

*ρ = [x<sub>1</sub> → v<sub>1</sub>,...,x<sub>n</sub> → v<sub>n</sub>]* means that variable name *x<sub>1</sub>* has value *v<sub>1</sub>* and so on.

The notation *ρ[x → v]* means the environment *ρ* is extended with a binding of *x* to value *v*.

#### Examples

They have the form:

<table>
	<tr>
		<td></td>
		<td>(e1)</td>
		<td>
			<em>
				<small>
				Premises here
				</small>
			</em>
		</td>
	</tr>
	<tr>
		<td>ρ |- i ⇒ i</td>
		<td></td>
		<td>
			<em>
				<small>
					Conclusion here
				</small>
			</em>
		</td>
	</tr>
</table>

In this example, the integer constant *i* evaluates to the integer *i*.

The rule is named (e1).

Another example:

<table>
	<tr>
		<td>ρ(x) = v</td>
		<td>(e3)</td>
		<td>
			<em>
				<small>
				Premises here
				</small>
			</em>
		</td>
	</tr>
	<tr>
		<td>ρ |- x ⇒ v</td>
		<td></td>
		<td>
			<em>
				<small>
					Conclusion here
				</small>
			</em>
		</td>
	</tr>
</table>

Here, the result of calling *ρ(x)* is *v*,
and variable *x* evaluates to the value of *v*.

Here's an example with multiple premises:

<table>
	<tr>
		<td>ρ |- e1 ⇒ v1</td>
		<td>ρ |- e2 ⇒ v2</td>
		<td>v = v<sub>1</sub> + v<sub>2</sub></td>
		<td>(e4)</td>
	</tr>
	<tr>
		<td>ρ |- e<sub>1</sub> + e<sub>2</sub> ⇒ v</td>
		<td></td>
		<td></td>
		<td></td>
	</tr>
</table>

It states that the expression *e<sub>1</sub> + e<sub>2</sub>* is evaluated by evaluation *e<sub>1</sub>* and *e<sub>2</sub>* separately, then adding the results.

#### Evaluation trees

Such evaluation rules may be used to build an evaluation tree. At the bottom of the tree, we find the conclusion, a judgement about the value *v* of some expression *e*. At the top, we find instances of rules that do not have premises.

## Static (lexical) scope and Dynamic Scope

### Static (lexical) scope

Static, or lexical, scope means that a variable occurrence refers to the *statically inner most enclosing* binding of a variable of that name.

This means, that just by looking at the program text, we can see which binding a given variable occurrence refers to.

For example:

```javascript
let y = 11;
function foo () {
	let y = 12;
}
```

Here, the declaration of `y` within `foo` *shadows* the one in the outer scope and is bound to *12*.

### Dynamic scope

With dynamic scope, a variable occurrence refers to the dynamically *most recent* binding of a variable of that name, no matter which scope it seems to be declared within.

## Adding types into the mix

We need a meta-language (`Type`). Here's a new typed language:

```fsharp
type Type =
	| TypeInt
	| TypeBool
	| TypeFunction of Type * Type;;

type TypedExpr =
	| CstI of int
	| CstB of bool
	| Var of string
	| Let string * TypedExpr * TypedExpr
	| Prim of string * TypedExpr * TypedExpr
	| if of TypedExpr * TypedExpr * TypedExpr
	| Letfun of string * string * Type * TypedExpr * Type * TypedExpr
	| Call of TypedExpr * TypedExpr;;
```