# Introduction to grammars and parsing in F#
> [Note on Grammars and parsing in F#](https://learnit.itu.dk/pluginfile.php/166124/course/section/88007/parsernotes.pdf)

### Tokenizer (Scanner)
A tokenizer (or scanner) divides a string or buffer into symbols (like identifiers, primitive values and operators)
and adds them chronologically to a collection. No parsing of their symbolic or semantical relation to one another is happening in this process.

### Abstract Syntax Tree (AST)
The end-result of the process of parsing.
It is a representation of a text which shows the *structure* of the text and leaves out irrelevant information such as the number of blanks between symbols.

### Grammar
A grammar is a set *T* of terminals, a set *N* of nonterminals, a set *R* of rules and a starting symbol *s belongs to N*.

#### Terminals
Symbols, e.g. *T = {"-", "0", "1"}*. The symbols the grammar include.

#### Nonterminal symbols
e.g. *N = {E, T}*

### Parsing problems (and solutions)
#### Left Factorization

Imagine a rule such as *E = T "-" E | T* where both alternatives start with the same symbol, *T*.
Here, the parser won't know which one to start with.

Using *Left factorization* you can arrive at an alternative version of the grammar:
*E = T ("-" E | epsilon)*:
*E = T Eopt .
Eopt = "-" T Eopt | epsilon .
T = "0" | "1"*.

Where *epsilon* is an empty sequence.
