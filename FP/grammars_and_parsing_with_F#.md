# Grammars and parsing with FSharp

> [Gammars and parsing](https://learnit.itu.dk/pluginfile.php/166124/course/section/88007/parsernotes.pdf)

While input to a program is given as text, it is generally better to represent it within the program in an abstract internal representation of the given expression.

## Grammars

The input descriptions are called *grammars*, and the programs for reading input are called *parsers*.

A *grammar G* is a **set of rules for combining symbols to a well-formed text**.

### Terminal symbols

The symbols that can appear in a text are called *terminal symbols*.

### Nonterminal symbols

Nonterminal symbols **cannot** appear in the final texts. Their only role is to help generating texts: strings of terminal symbols.

### Grammar rules

A *grammar rule* has the form *A = f<sub>1</sub> | ... | f<sub>n</sub>* where the *A* on the left-hand side is the nonterminal symbol defined by the rule and the *f<sub>i</sub>* on the right-hand side show the legal ways of deriving a text from the nonterminal *A*.

#### Alternatives

Each *alternative* `f` is a *sequence e<sub>i</sub> ... e<sub>m</sub>* of symbols.

##### The empty sequence

We write `Λ` for the empty sequence (that is, when `m = 0`).

### Symbols

A *symbol* is either a *nonterminal symbol A* defined by some grammar rule, or a *terminal symbol* "c" which stands for `c`.

#### Starting symbol

The *starting symbol S* is one of the nonterminal symbols.

### Derivation

The grammar rule *T = "0" | "1"* says that we may derive either the string "0" or the string "1" from the nonterminal *T*, by replacing or substituting either "0" or "1" for *T*.

These are called *derivations* and are written: *T ⇒ "0"* and *T ⇒ "1"*.

### Grammar TL;DR

 A grammar can be used to derive symbol strings having a certain structure.

## Parsing theory

With parsing, we get **some** input text and a grammar, and the task is to:

1. See whether the text *could* have been generated by the grammar.
2. If it could, see *how* it was generated, e.g. which grammar rules and which *alternatives* were used in the derivation.

This process is called *parsing* or *syntax analysis*.

When are done, we know *if* the input string is derivable from the starting symbol as well as at least one *way* it can derived.

## Parser construction in FSharp

We can represent the input string as a *list* of terminal symbols, where the terminal symbols belong to a suitable datatype `terminal`.

A simple terminal symbol such as "-" may be represented by a parameterless constructor such as `Sub`.

### Scanners

A scanner has type `scan : string -> terminal list`. The process of scanning an input text can also be called *lexical analysis*.

### Distinguishing names from keywords

When something looking like an identifier/name is happened upon, remember to check it against a list of keywords and reserved names. If it is any of those, use another type for it.

There's a lot of interesting stuff in here, and definitely something worth going back to, but at this point in time I don't have time to delve more into the topic.