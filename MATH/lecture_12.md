# Lecture 12

> Discrete Mathematics and Its Applications, Chapter 13.4, 13.5

## Regular Sets

Regular sets are sets that can be built up from the null set, the empty string, and singleton string by taking concatenations, unions and Kleene closures.

A set is regular if and only if it is generated by a regular grammar.

### Regular Expressions

The *regular expressions* over a set *I* are defined recursively by:

- The symbol *∅* (the empty set) is a regular expression.
- The symbol *λ* (the empty string) is a regular expression.
- The symbol *x* is a regular expression whenever *x ∈ I*
- The symbols *(**AB**), (**A** ∪ **B**)*, and ***A∗*** are regular expressions whenever ***A*** and ***B*** are regular expressions.

***(AB)*** represents the concatenation of the sets represented by ***A*** and ***B***.

(**A** ∪ **B**) represents the union of the sets represented by ***A*** and ***B***.

***A∗*** represents the Kleene closure of the set represented by ***A***.

## "Problems" with Finite-state Automata

The main problem is that they are unable to carry out many computations. The main limitation of these machines is their finite amount of memory! Thus, they can only recognize languages that are regular.

## Alternatives to Finite-State Automata

Because of the limits of finite-state machines, we need to look at more powerful models of computation if we want to go further than regular languages.

One such model is the *pushdown automaton*

### Pushdown Automata

A Pushdown Automaton includes everything in a finite-state automaton, as well as a stack, which provides unlimited memory!

Symbols can be placed on the top or taken off the top of the stack.

### Linear Bounded Automata

A linear bounded automaton is more powerful than a pushdown automaton. In particular, it can recognize context-sensitive languages. However, these machines cannot recognize all the languages generated by phrase-structure grammars.

Another machine avoids these limitations. Enter: a *Turing Machine*

## Turing Machines

A Turing Machine is made up of everything included in a finite-state machine together with a tape, which is infinite in both directions.

A Turing Machine has *read* and *write* capabilities on the tape, and it can move back and forth along this tape.

A Turing Machine can recognize all languages generated by phrase-structure grammars. And, it can model all the computations that can be performed on a computing machine.

So, while a finite-state automaton is easy to understand, it cannot be used as a general model of computation. It is too limited in what it can do (it can only recognize regular sets).

A Turing machine consists of:

- A *control unit* (head), which at any step is in one of finitely many different states
- A tape, divided into cells, which is infinite in both directions.

A Turing Machine has read and write capabilities on the tape as the control unit (head) moves back and forth along this tape, changing states depending on the tape symbol read.

**Turing Machines are much nore powerful than real computers, which have finite memory capabilities**!

### Definition of Turing Machines

A *Turing Machine T = (S, I, f, s<sub>0</sub>)* consists of a finite set *S* of states, an alphabet *I* containing the blank symbol *B*, a partial function *f* from *S x I* to *S x I x {R, L}* and a starting state *s<sub>0</sub>*.

The machine also needs a *halt* state. Otherwise, the machine will keep looping forever and never complete.

If the machine ends in a *halt* state, it stops. Whatever computation we're after will most likely have been written to tape. That depends on the state machine provided to the Turing Machine.