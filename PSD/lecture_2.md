# Lecture 2

> Mogensen BCD sections 2.1-2.7, 2.9, 3.1-3.6
> Programming Language Concepts, Chapter 3

## Lexical Analysis

Lexical means *pertaining to words*.

### Tokens

Words are objects like variable names, numbers, keywords, etc.
We call such words *tokens*.

### Lexical Analyser ("lexer")

A *lexer* will as its input take a string of individual letters and divide this string into tokens.

It will also filter out whatever separates the tokens (the so-called *white-space*) as well as comments.

### Purpose of the lexical analysis

The main purpose is to make life easier for the subsequent syntax analysis phase.

### How a lexer works in simple terms

1. You first read past initial white-space.
2. Then you, in sequence, test to see if the next token is a keyword, a number, a variable or whatnot.

These can be difficult to maintain and write. Therefore, people often use *lexer generators* which transform human-readable *specifications* of tokens and white-space into efficient programs.

### Lexer specifications

You typically write specifications using regular expressions.

## Nondeterministic Finite Automata

### Finite Automaton

A *finite automaton* is a machine that has a finite number of *states* and a finite number of *transitions* between these.

Such transitions are usually labelled by a character from the input alphabet, but they can also be marked with *ε*, the so-called *epsilon transitions*.

**A finite automaton can be used to decide if an input string is a member in some particular set of strings**. To do this, we select one of the states of the automaton as the *starting state*.

We start there, and then in each step we can either:

- Follow an epsilon transition to another state, or
- Read a character from the input and follow a transition labelled by that character.

When all characters has been read from the input, we check if the current state is marked as being *accepting*, and if it is, the string we read from the input is in the language defined by the automaton - we're good!

**We may have several actions to take at each step! We can either go for an epsilon transition or a transition on an alphabet character. If this is the case, the automaton is *nondeterministic***!

### Definition of Nondeterministic Finite Automaton

A nondeterminstic finite automaton consists of a set *S* of states. One of these states, *s<sub>0</sub> ∈ S* is called the *starting state* and a subset *F ⊆ S* of the states are accepting states. Additionally we have a set *T* of transitions. Each transition *t* connects a pair of states *s<sub>1</sub>* and *s<sub>2</sub>* and is labelled with a symbol, which is either a character *c* from the alphabet *Σ* or the symbol *ε*, which indicates an epsilon-transition. A transition from state *s* to state *t* on the symbol *c* is written as *s<sup>c</sup>t*.

### Abbreviation for Nondeterministic Finite Automaton

NFA

### Accepting vs Final states

They are the same thing. It is just two names for the same thing.
The set of accepting/final states are called *F*.

### Efficiency of Nondeterministic Finite Automaton

They aren't that efficient since determining whether or not a string will be accepted by an NFA will require checking all possible paths of the NFA. Instead, Deterministic Finite Automaton, where there is a maximum of one possible path from a state, is more efficient and should be used for efficiency.

### Going from a Regular Expression to an NFA

It's all about dividing the Regular Expression into subexpressions and constructing NFA fragments from them before finally combining these fragments into bigger fragments and then the final NFA.

## Deterministic Finite Automata (DFA)

A DFA is closer to "the machine" as NFA's and much more efficient. It's also way more restricted:

- There are no epsilon-transitions
- There may *not* be two identically labelled transitions out of the same state.

**This means that we never have a choice of several next-states**! The state and the next input symbol uniquely determine the transition - or lack of the same. **This is why these automata are called *deterministic***.

**Any NFA can be converted to an equivalent DFA**!

### Lexers and Finite Automata

Most lexical analysers are based on DFAs because of their efficiency.

### Lexers and Lexer Generators

A NFA/DFA/RegExp is not enough on its own to perform lexical analysis.

We need to be able to distinguish between a multitude of different types of tokens such as numbers, variables and keywords. And each of those are described by its own regular expression.

And, a lexer does not check if the entire input is included in the languages defined by the regular expressions. Instead, it has to cut it into pieces - tokens - each of which is included in one of the languages (hopefully, otherwise the syntax is faulty).

### Precedence and overlaps between tokens in Lexers

There may be overlaps. For example, variables may have the same names as keywords. You, as the implementor of the lexer specification will decide what to do with such overlaps. Will you throw an exception or will you allow overwriting it?

Precedence rules comes into play here. Normally, the order in which tokens are defined in the input to the lexer generator indicates priority. Meaning, earlier defined tokens take precedence over later defined tokens. So, keywords would be defined before variable names, which menas that the string *if* will be recognised as a keyword before it is recognized as a variable name.

### Lexical errors

If no prefix of the input string forms a valid token, we're having a lexical error. We can be forgiving and continue the analysis by skipping characters until a valid prefix is found or we can throw an error.

## Lexer generators

### Input to Lexer generators

The *input* will normally contain a list of regular expressions that each denote a token. Each of these regular expressions has an associated *action*.

#### Actions

The action describes what is passed on to the consumer - the parser. This will typically be some data type that describes that type of token. For example, what kind of token it is, its position, etc.

Such an action may also include information about its current nesting level, etc. For example, for nested comments, which a regular expression cannot recognize arbitrarily, we can use a global counter.

## Syntax Analysis (Parsing)

Where lexical analysis splits the input into tokens, the purpose of syntax analysis (pr *parsing*) is to **recombine these tokens into an Abstract Syntax Tree (AST) that reflects the structure of the text**.

The leaves of the tree are the tokens found by the lexical analysis, and if the leaves are read from left to right, the sequence is the same as in the input text.

Normally, the same thing happens here as with lexical analysis - A parser *specification* is written in anotation suitable for human understanding, and then it is transformed into a machine-like low-level notation suitable for efficient execution. This process is called *parser generation*. We use *context-free-grammars* for writing these parser specifications.

### Context-free Grammars

This is a recursive notation for describing sets of strings and imposing a structure on each such string in the language it defines.

#### Language

A language is defined over some alphabet. For example, the set of tokens produced by a lexer, or the set of alphanumeric characters.

#### Terminals

The symbols in the alphabet are called *terminals*.

#### Nonterminals

A context-free grammar recursively defines several **sets of strings**. Each set is denoted by a name, which is called a *nonterminal*. This has nothing to do with the set of terminals.

#### Start symbol

One of the nonterminals are chosen to denote the language described by the grammar. This is called the *start symbol* of the grammar.

#### Productions

The sets are described by a number of *productions*. Each production describes some of the possible strings that are contained in the set denoted by a nonterminal.

It has the form:

*N → X1 ... X<sub>n</sub>*

Where *N* is a nonterminal and *X* is a symbol, **each of which is either a terminal or a nonterminal**.

For example:

*A → a* says that the set denoted by the nonterminal *A* contains the one-character string `a`.

##### Productions with empty right-hand sides

These are called *empty productions*. For example, check this grammar:

*B →*
*B → `a`B*

Here, there first production indicates that the empty string is part of the set *B*.

You could also place and *ε* at the right-hand side instead of leaving it empty to be more specific.

##### Multiple productions of the same nonterminal combined

You can combine multiple productions of the same nonterminal in one rule by separating them with `|`:

*T → R|`a`T`a`*
*R → `b`|`b`R*

is shorthand for:

*T → R*
*T → `a`T`a`*
*R → `b`*
*R → `b`R*

##### Context-free grammar examples

Here's an example where the nonterminal in question is of type `expr`:

```text
expr -> expr + expr
expr -> expr - expr
expr -> expr * expr
expr -> expr / expr
expr -> num
expr -> (expr)
```

This is a way to declare which productions are "allowed" for the nonterminal with name *expr*.

### Syntactic categories

When writing a grammar for a programming language, one normally starts by dividing the constructs of the language into different syntactic categories. This is a sub-language that embodies a particular concept. For example:

|        Name        |                           Description                           |
|--------------------|-----------------------------------------------------------------|
|   **Expressions**  |            are used to express calculation of values            |
|   **Statements**   |       express actions that occur in a particular sequence       |
|  **Declarations**  |  express properties of names used in other parts of the program |

Each of these categories is denoted by a main *nonterminal*. For example, `expr` could have been named *Expression* like in the above table.

We can then create as many sub-languages of syntactic categories as we like. And, statements may contain expressions and vice-versa.

## Operator Precedence

For an expression such as: `2 + 3 * 4`, without precedence rules, we would just read it from left to right or from right to left, but we want precedence rules to dictate that the product must be calculated before the sum, for example.

The way we do so may be to define precedence rules during syntax analysis (parsing) to select among the possible syntax trees.

Another way to do so is to express the operator hierarchy in the grammar itself.

### Left-associate operators

An operator (*⊕*) is left-associate if the expression *a ⊕ b ⊕ c* must be evaluated from left to right.

Specifically, as *(a ⊕ b) ⊕ c*.

### Right-associate operators

An operator (*⊕*) is right-associative if the expression *a ⊕ b ⊕ c* must be evaluated from right to left.

Specifically, *a ⊕ (b ⊕ c)*.

### Non-associative operators

An operator (*⊕*) is non-associative if expressions of the form *a ⊕ b ⊕ c* **are illegal**.

### Standard practices for associativity for operators

Usually, `-` and `/` are left-associate, while `+` and `*` are mathematically associative meaning that it does not matter if we calculate from left to right or vice-versa. We usually make them left-associative to avoid ambiguity, though.

### Derivation

Derivation is simply replacing nonterminals with the right-hand side of any production where the nonterminal is found on the left-hand side.

We can repeat this until we only have terminals left. At that point, what we have is a string in the language defined in the grammar.

So, in other words, this is the process of "printing" the AST into the language it represents. Gradually, by replacing nonterminals along the way.

### Syntax Analysis (parsing)

The syntax analysis phase of a compiler will take a string of tokens produced by the lexer (or *tokenizer*) and from this construct a syntax tree for the string by finding a derivation of the string from the start symbol of the grammar.

## `fslex` and `fsyacc`

In this course, the lexer generator is called *fslex* and the parser generator is called *fsyacc*.

The reasons for this is that the classical lexer and parser generators for C are called *lex* and *yacc*.

## Bottom-up parsers (LR parsers)

Bottom-up parsers are ones that are characterized by reading characters from the left and making derivations from the right-most nonterminal.

This is why they are called LR parsers - Reading from the Left (L) and deriving from the right (R).

## Top-down parsers (LL parsers)

These are ones that read from the left - but also derives from the left-most nonterminal.

## Context-free grammars for `fslex`

Has four components:

- *terminal symbols* such as identifiers, integers, string constants, operators, keywords, etc.
 *nonterminal symbols*, denoting abstract syntax classes (such as `expr`, `Stmt`, etc)
- a *start symbol S*
- *grammar rules (productions)* of the form: *A ::= tnseq* where *tnseq* is a sequence of terminal or nonterminal symbols.

For example:

```text
Expr ::= NAME
      |  INT
			|  - INT
			|  ( Expr )
			|  let NAME = Expr in Expr end
			|  Expr * Expr
			|  Expr + Expr
			|  Expr - Expr
```

Usually, the start symbol is also declared as: `Main ::= Expr EOF` where *EOF* means *End Of File*.

### `%left` declaration on symbols

This indicates that specific operators should be left-associative.

### The `Absyn.fs` file

This is where we declare our nonterminals (the Abstract Syntax classes).

### Important files

- `Expr/Absyn.fs` is where abstract syntax (nonterminals) are declared
- `Expr/ExprLex.fsl` is where the lexer specification is located. **This is the input to `fslex`**.
- `Expr/ExprPar.fsy` is where the parser specfication is. **This is the input to `fsyacc`**.
- `Expr/Parse.fs` is where the expression parser is declared.

## `ExprPar.fsy`

In the top of the file, the tokens are declared:

```fsy
%token <int> CSTINT
%token <string> NAME
%token PLUS MINUS TIMES EQ
%token END IN LET
%token LPAR RPAR
%token EOF
```

Then, the operator associativity and precedence is given:

```fsy
%left MINUS PLUS /* lowest precedence */
%left TIMES /* highest precedence */
```

Then, the start symbol is declared:

```fsy
%start Main
%type <Absyn.expr> Main
%%
```

Notice it is given the nonterminal *Main* as the start symbol

Now comes the rules, which is the grammar:

```fsy
Main:
		Expr EOF { $1 }
Expr:
		NAME 														{ Var $1 }
		| CSTINT 												{ CstI $1 }
		| MINUS CSTINT 									{ CstI (- $2) }
		| LPAR Expr RPAR 								{ $2 }
		| LET NAME EQ Expr IN Expr END 	{ Let($2, $4, $6) }
		| Expr TIMES Expr 							{ Prim("*", $1, $3) }
		| Expr PLUS Expr 								{ Prim("+", $1, $3) }
		| Expr MINUS Expr 							{ Prim("-", $1, $3) }
```

The first rule says that to parse a `Main`, one must parse an `Expr` followed by an `EOF` (end of file).

The expressions in curly braces on the right described what result will be produced by a successful parse (the *semantic actions* for the AST).

So, the left-hand-side says, for example, "If you see an Expr, followed by a PLUS, followed by an Expression, we're having to do with the abstract syntax class `Prim`". Notice how the capture groups are then passed on to the type constructor within the curly braces.