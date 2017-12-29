# Lecture 1 and 2

> Discrete Mathematics and Its Applications, Chapter 1

## Proofs

The thing that makes up a correct mathematical argument is a *proof*.

## Theorem

Once we prove a mathematical statement is true, we call it a *theorem*.

## Topic

A topic is a collection of related theorems.

## Propositional Logic/Propositional Calculus

A *proposition* is a sentence that declares a fact that is either **true or false, but not both**. We call them *declarative sentences*.

For example:

`1 + 1 = 2`
`Copenhagen is the capital of Denmark`
`2 + 2 = 5`

The first two are true while the last one is false. All of them are propositions.

Now consider this:

`x + 1 = 2`

This is **not** a proposition because **it is neither true or false**! We can turn it into one by exchanging the value of *x* with something else that makes it true or false.

## Propositional variables

Also called *Statement variables*. This is **variables that represent propositions**.

We typically use the letters *p, q, r, s*.

### Truth value

The truth value of a proposition is denoted by `T` if it is a true proposition.

The truth value of a proposition is denoted by `F` if it is a false proposition.

### Compound Propositions

A *Compound Proposition* is a new proposition, e.g. a mathematical statement formed from existing propositions using logical operators.

## Operators

An *operator* is something that can construct a new proposition from one or more existing propositions.

## Negation

Let *p* be a proposition. The *negation of p* is denoted by `¬p` and is the statement:

*"It is not the case that p"*.

It is read: *not p*.

The `¬` symbol is an operator. It can construct a new proposition from a single existing proposition - its negation.

## Truth table

A *truth table* has a row for each possible truth value of a proposition *p*.

A proposition *p* can be either true (*T*) or false (*F*). Depending on its truth value, its negation will have the opposite one:

| *p* | *¬p*  |
|-----|-------|
|  T  |   F   |
|  F  |   T   |

## Connectives

Connectives are logical operators such as `¬` (negation).

## Conjunction

The *conjuction* of *p* and *q* is the proposition:

*p AND q*.

It has the operator: *∧*.

It is denoted: *p ∧ q*.

It is true (*T*) when both *p* and *q* are true and is false (*F*) otherwise.

Here's a truth table for *p ∧ q*:

|    p    |    q    |   p ∧ q   |
|---------|---------|-----------|
|    T    |    T    |     T     |
|    T    |    F    |     F     |
|    F    |    T    |     F     |
|    F    |    F    |     F     |

### Using *but* in place of *and*

Sometimes in language, we say *but* even though we mean *and*:

*"The sun is shining, but it is raining"* is logically equivalent to: *The sun is shining AND it is raining*.

## Disjunction

The *disjuction* of *p* and *q* is the proposition:

*p OR q*.

It has the operator: *∨*.

It is denoted: *p ∨ q*.

It is false (*F*) when both *p* and *q* are false and is true (*T*) otherwise.

Here's a truth table for *p ∨ q*:

|    p    |    q    |   p ∨ q   |
|---------|---------|-----------|
|    T    |    T    |     T     |
|    T    |    F    |     T     |
|    F    |    T    |     T     |
|    F    |    F    |     F     |

## Inclusive OR vs Exclusive OR

- Inclusive or (OR) is the one that states that the compound proposition is true if either one of the propositions is true or both of them are true.

Example:

*Students who have taken calculus or computer science can take this class*.

- Exclusive or (XOR) states that the compound proposition is true if either one of the propositions is true but NOT both of them.

An XOR has the symbol: `⊕`.

The XOR between two propositions is denoted: `p ⊕ q`.

Example:

*Students whp have taken calculus or computer science, but not both, can take this class*.

Here's a truth table for an exclusive OR (XOR):

|   *p*   |   *q*   |  *p ⊕ q*  |
|---------|---------|-----------|
|    T    |    T    |     F     |
|    T    |    F    |     T     |
|    F    |    T    |     T     |
|    F    |    F    |     F     |

## Conditional Statements

A *conditional statement* uses the operator `→`.

It is also called an *implication*.

The conditional statement `p → q` means:

*If p, then q*.

As such, `p → q` is false when *p* is true and *q* is false, and true otherwise.

`p → q` asserts that *q* is true on the condition that *p* holds.

If the condition is not met, (e.g. if *p* is false), *q* may still hold - its just that we cannot say that *q* is definetly gonna happen. **So, if *p* is false, `p → q` is always true**.

Here's a truth table for conditional statements:

|   *p*   |   *q*   | *p → q* |
|---------|---------|----------|
|    T    |    T    |    T     |
|    T    |    F    |    F     |
|    F    |    T    |    T     |
|    F    |    F    |    T     |

You have to be very careful to remember, that if *p* is false and *q* is false, then the expression will still be true, since the hypothesis is false and the *conclusion* is never met.

### Hypothesis and conclusion

In `p → q`, *p* is the **hypothesis** and *q* is the **conclusion**.

### Textual expressions of conditional statements

All of these mean the same thing:

*"if p, then q"*
*"p implies q*
*"q follows from p*
*"if p, q"*
*"p is sufficient for q"*
*"q if p"*
*"q when p"*
*"a neccesary condition for q is p"*
*"p only if q*

## Assignment

The symbol `:=` stands for *assignment*. The statement:
`x := x + 1` means "assign *x + 1* to *x*".

## Converse

The proposition `q → p` is called the *converse* of *p → q*.

## Contrapositive

The *contrapositive* of *p → q* is the proposition *¬q → ¬p*  .

Notice how both sides of the expression is negated but their order is also flipped.

**This will always have the same truth value as *p → q*!**.

We say that a **conditional statement and its contrapositive are equivalent**.

## Inverse

The *inverse* of *p → q* is *¬p → ¬q*.

## Biconditionals

The *biconditional* statement `p ↔ q` is true when *p* and *q* have the same truth values - and is false otherwise!

It could also be expressed:

*(p → q) ∧ (q → p)* (p implies q and q implies p).

Here's a truth table for the Biconditinal statement `p ↔ q`:

|   *p*   |   *q*   | *p ↔ q* |
|---------|---------|----------|
|    T    |    T    |     T    |
|    T    |    F    |     F    |
|    F    |    T    |     F    |
|    F    |    F    |     T    |

For example:

*"You can take the flight if and only if you buy a ticket"* is a biconditional statement. There's no way you would be able to take the flight without the ticket.

## Building up Truth Tables for compound propositions

We can connect as many propositions we want to form compound propositions. Building truth tables for these is done by splitting the compound proposition up in its parts.

**The amount of rows is dependent on the amount of propositional variables**!

For example, if there are two propositional variables, *p* and *q*, there will be four rows - one for each pair of truth values.

For example, for the compound proposition:

*(p ∨ ¬q) → (p ∧ q)*.

The Truth table would be:

|   *p*   |   *q*   |  *¬q*  | *p ∨ ¬q* |  *p ∧ q*  |*(p ∨ ¬q) → (p ∧ q)*|
|---------|---------|--------|----------|-----------|---------------------|
|    T    |    T    |    F   |    T     |     T     |          T          |
|    T    |    F    |    T   |    T     |     F     |          F          |
|    F    |    T    |    F   |    F     |     F     |          T          |
|    F    |    F    |    T   |    T     |     F     |          F          |

## Logical operator Precedence

We generally use parentheses to specify the order in which logical operators in a compound proposition are to be applied. But, there are precedence rules in place in case no parenthesis is applied.

|   Operator   |  Precedence  |
|--------------|--------------|
|      ¬       |       1      |
|      ∧       |       2      |
|      ∨       |       3      |
|      →      |       4      |
|      ↔      |       5      |

As you can see, negation is of highest precedence. Conjunction (*AND*) is of higher precedence than discjunction (*OR*).

## Bit Operations

Computers represent information using bits.

### Bit

A *bit* is a symbol with two possible values:

- 0 (zero)
- 1 (one)

There is a truth value associated with these:

- 1 is true (*T*)
- 0 is false (*F*)

By replacing *true* by a *1* and a *false* by a *zero*, we can perform the logical connecties with computer bit operations.

## Propositional Equivalences

### Tautology

A *tautology* is a compound proposition that is **always true, no matter the truth values of the propositional variables that occur in it**.

For example:

*p ∨ ¬p* is a tautology. It is always true, no matter the value of the propositional variable *p*.

We can use a truth table to illustrate this:

|   *p*   |   *¬p*   | *p ∨ ¬p* |
|---------|----------|----------|
|    T    |     T    |    T     |
|    T    |     F    |    T     |
|    F    |     T    |    T     |
|    F    |     F    |    T     |

### Contradiction

A *contradiction* is a compound proposition that is **always false**.

For example:

*p ∧ ¬p* is a contradiction. It is always false, no matter the value of the propositional variable *p*.

We can use a truth table to illustrate this:

|   *p*   |   *¬p*   | *p ∧ ¬p* |
|---------|----------|----------|
|    T    |     T    |    F     |
|    T    |     F    |    F     |
|    F    |     T    |    F     |
|    F    |     F    |    F     |

### Contingency

A *contingency* is a compound proposition that **is neither a tautology nor a contradiction**.

### Logical Equivalence

Uses the symbol: `≡`.

`p ≡ q` Denotes that two propositions *p* and *q* are logically equivalent.

**They will be so if *p ↔ q* is a tautology**!

## De Morgan's Laws

These are are useful for logical equivalence:

*¬(p ∧ q) ≡ ¬p ∨ ¬q*.
*¬(p ∨ q) ≡ ¬p ∧ ¬q*.

These tell us how to **negate conjunctions and disjunctions**.

## Propositional Satisfiability

A compound proposition is *satisfiable* **if there is an assignment of truth values to its variables that makes it true**.

If there is no such value, **the compound proposition is *unsatisfiable*!**.

If there is, any of such values would be a *solution* to the proposition.

## Predicates and Quantifiers

Predicate logic is a more powerful type of logic.

### Predicates

Consider the statement:

*x is greater than 3*.

Here, the first part, *x* is the variable. The rest, *is greater than 3*, is the **predicate**.

A predicate is a property that the subject of the statement can have.

We can denote a predicate with *P*.
In the example above, *P* denotes the predicate *is greater than 3*, and we could write the entire statement as such:

*P(x)*.

Once a value has been assigned to *x*, the statement becomes a proposition (because it now has a truth value).

For example, for the statement `P(x)` where `P = is greater than 3`, *P(4)* is true (since 4 is greater than 3) and P(3) is false (since 3 is not greater than 3).

## Preconditions and postconditions

### Preconditions

*Preconditions* describe valid input.

### Postconditions

*Postconditions* describe the conditions that the output should satisfy after the program has run.

### Quantifiers

A *quantifier* expresses the extent to which a predicate is true over a range of elements.

For example, in **universal quantification**, we say that a predicate is true for *every* element under consideration.

And, in **existential quantification**, we say that there is one or more elements under consideration for which the predicate is true.

This is called **predicate calculus**.

### The universal quantifier

When a mathematical statement asserts that a property is true **for all values of a variable in a particular domain (*"the domain/universe of discourse"*), we can express this using **universal quantification**.

This can be expressed in written text:

*P(x) for all values of x in the domain*.

And is mathematically expressed:

*∀xP(x)*.

*∀* is called the *universal quantifier*. We read it as: "*for every xP(x)*".

Such one may be true or false.
For example:

Let *P(x)* be the statement *x + 1 > x*.

Here, the quantification *∀xP(x)* is true, since *P(x)* is true for all real numbers.

But, in this example:

Let *Q(x)* be the statement *x < 2*.

Here, the quantification *∀xQ(x)* is false, since *Q(x)* is not true for all real numbers.

### The existential quantifier

This can be expressed in text:

*There is an x for which P(x) is true*.

And is mathematically expressed:

*∃xP(x)*.

Such one may be true or false.
For example:

Let *Q(x)* be the statement *x < 2*.

Here, the quantification *∃xQ(x)* is true, since there exists a real number for which *Q(x)* is true (for example, 1).

### The uniqueness quantifier

This one is denoted by *∃!* or *∃1<sub>1</sub>*.

It states: *There exists a unique x such that P(x) is true* or *There is one and only one x for which P(x) is true*.

## Quantifiers with Restricted Domains

Often the domain of a quantifier needs to be restricted.

For example:

*∀x < 0(x<sup>2</sup> > 0)*.

means: *"for every x where x < 0, x<sup>2</sup> > 0"*.
In other words, the square of a negative real number is positive.

It could also be expressed as:

*∀x(x < 0 → x<sup>2</sup> > 0)*. (For every x, if x is < 0, then x squared is greater than zero).

We could also use this with the existential quantifier:

*∃x > 0 (x<sup>2</sup> = 2)* (There exists an x that is greater than zero for which x squared is equal to 2).

### Binding variables

If a quantifier is used on a variable, we say that this occurence of the variable is **bound**.

For example, in *∃x > 0 (x<sup>2</sup> = 2)*, *x* is bound by the existential quantification *∃x*.

Otherwise, the variable is **free**.

### Negating Quantified Expressions

If we have a universal quantification:

*∀xP(x)*.

With the text: "Every student in your class has taken a course in calculus".

Its negation will be "It is not the case that every student in your class has taken a course in calculus" which is equivalent to "There is a student in your class who has not taken a course in calculus". Thus, it can be expressed by the simple existential quantification:

*∃x ¬P(x)*.

### De Morgan's Laws for Quantifiers

|      Negation      | Equivalent Statement |           When is Negation true?           |                When False?                |
|--------------------|----------------------|--------------------------------------------|-------------------------------------------|
|     *¬∃xP(x)*      |       *∀x¬P(x)*      |        For every *x*, *P(x)* is false      |  There is an *x* for which *P(x)* is true |
|     *¬∀xP(x)*      |       *∃x¬P(x)*      |  There is an *x* for which *P(x)* is false |       *P(x)* is true for every *x*        |

## Nested Quantifiers

A quantifier can be within the scope of another.

For example:

*∀x∃y(x + y = 0)*.

Which says that for every real number *x* there is a real number *y* such that *x + y = 0*. In other words, every real number has an additive inverse.

**A good way to think about nested quantifiers is by thinking of them in terms of nested loops**!

For example:

*∀x∀yP(x,y)* denotes: *"For all real numbers x, for all real numbers y, P(x)*.

## Rules of Inference

Rules of inference are template for constructing valid arguments which we use to deduce new statements from statements we already have.

### Proofs (again)

A proof is a sequence of valid arguments that establish the truth of mathematical statements.

By *valid*, it is meant that the conclusion, or final statement of all of the arguments, must follow from the truth of the preceding statements

### Arguments

An *argument* is a sequence of statements that end with a conclusion.

An argument is valid if and only if it is impossible for all the premises to be true and the conclusion to be false.

## Valid arguments in Propositional Logic

An example of an argument (which is a sequence of propositions) is:

*"If you have a current password, then you can log onto the network.*

*You have a current password.*

*Therefore,*

*You can log onto the network"*.

To determine whether or not it is valid - in other words, whether the conclusion *"You can log onto the network"* is true - We must look at the validity of the preceding premises.

We can break this down into propositions:

*p* can represent *"You have a current password"* and *q* can represent *"You can log onto the network"*.

Looking at the premises, we say that p implies q (if p, then q), written *p → q*.

Hence the arguments above becomes:

|                                Text                                  |  Proposition  |
|----------------------------------------------------------------------|---------------|
|  If you have a current password, then you can log onto the network.  |    *p → q*   |
|                   You have a current password                        |      *p*      |
|                   You can log onto the network                       |      *q*      |

We can write this more formally:

<table>
	<tr>
		<td></td>
		<td style="font-style:italic">p → q</td>
	</tr>
	<tr>
		<td></td>
		<td style="font-style:italic">p</td>
	</tr>
	<tr>
		<td style="font-weight:bold; background: rgba(255,255,255,.1)">∴</td>
		<td style="font-style:italic; font-weight: bold; background: rgba(255,255,255,.1)">q</td>
	</tr>
</table>

*∴* means: "therefore".

We can conclude that this form of argument is valid because whenever all of its premises are true, the conclusion must also be true!

### The most important Rules of Inference

The Rules of Inference are some relatively simple argument forms.

#### Modus Ponens/The Law of Detachment

Builds on the tautology:

*(p ∧ (p → q)) → q*.

This leads to the argument form already covered, for example:

<table>
	<tr>
		<td></td>
		<td style="font-style:italic">p</td>
	</tr>
	<tr>
		<td></td>
		<td style="font-style:italic">p → q</td>
	</tr>
	<tr>
		<td style="font-weight:bold; background: rgba(255,255,255,.1)">∴</td>
		<td style="font-style:italic; font-weight: bold; background: rgba(255,255,255,.1)">q</td>
	</tr>
</table>

Here's a full table of the most important rules of inference:

|  Rule of inference                             |             Tautology             |             Name            |
|------------------------------------------------|-----------------------------------|-----------------------------|
|<table><tr><td></td><td style="font-style:italic">p</td></tr><tr><td></td><td style="font-style:italic">p → q</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">q</td></tr></table>| *(p ∧ (p → q)) → q* | Modus ponens |
|<table><tr><td></td><td style="font-style:italic">¬q</td></tr><tr><td></td><td style="font-style:italic">p → q</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">¬p</td></tr></table>| *(¬q ∧ (p → q)) → ¬p* | Modus tollens |
|<table><tr><td></td><td style="font-style:italic">p → q</td></tr><tr><td></td><td style="font-style:italic">q → r</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">p → r</td></tr></table>| *((p → q) ∧ (q → r)) → (p → r)* | Hypothetical syllogism |
|<table><tr><td></td><td style="font-style:italic">p ∨ q</td></tr><tr><td></td><td style="font-style:italic">¬p</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">q</td></tr></table>| *((p ∨ q) ∧ ¬p) → q* | Disjunctive syllogism |
|<table><tr><td></td><td style="font-style:italic">p</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">p ∨ q</td></tr></table>| *p → (p ∨ q)* | Addition |
|<table><tr><td></td><td style="font-style:italic">p ∧ q</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">p</td></tr></table>| *(p ∧ q) → p* | Simplification |
|<table><tr><td></td><td style="font-style:italic">p</td></tr><tr><td></td><td style="font-style:italic">q</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">p ∧ q</td></tr></table>| *((p) ∧ (q)) → (p ∧ q)* | Conjunction |
|<table><tr><td></td><td style="font-style:italic">p ∨ q</td></tr><tr><td></td><td style="font-style:italic">¬p ∨ r</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">q ∨ r</td></tr></table>| *((p ∨ q) ∧ (¬p ∨ r)) → (q ∨ r)* | Resolution |

## Using Rules of Inference to Build Arguments

When there are many premises, often we need several rules of inference to show that an argument is valid.

For example:

For the premises:

"It is not sunny this afternoon and it is colder than yesterday"
"We will go swimming only if it is sunny"
"If we do not go swimming, then we will take a canoe trip"
"If we take a canoe trip, then we will be home by sunset"
"We will be home by sunset"

We can divide them into propositions:

Let *p* be the proposition "It is sunny this afternoon".
Let *q* be the proposition: "It is colder than yesterday".
Let *r* be the proposition: "We will go swimming"
Let *s* be the proposition: "We will take a canoe trip"
Let *t* be the proposition (and conclusion): "We will home by sunset"

Now we have:
*¬p ∧ q, r → p, ¬r → s, and s → t*.

In steps:

| Step number |    Step    |                Reason                |
|-------------|------------|--------------------------------------|
|      1      |  *¬p ∧ q*  |                Premise               |
|      2      |    *¬p*    |        Simplification using (1)      |
|      3      |  *r → p*  |                Premise               |
|      4      |    *¬r*    |    Modus tollens using (2) and (3)   |
|      5      |  *¬r → s* |                Premise               |
|      6      |     *s*    |    Modus ponens using (4) and (5)    |
|      7      |  *s → t*  |                Premise               |
|      8      |     *t*    |    Modus ponens using (6) and (7)    |

## Rules of Inference for Quantified Statements

|  Rule of inference                             |             Name            |
|------------------------------------------------|-----------------------------|
|<table><tr><td></td><td style="font-style:italic">∀xP (x)</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">P(c)</td></tr></table>| Universal instantiation |
|<table><tr><td></td><td style="font-style:italic">P(c) for an arbitrary c</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">∀xP(x)</td></tr></table>| Universal generalization |
|<table><tr><td></td><td style="font-style:italic">∃xP(x)</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">P(c) for some element c</td></tr></table>| Existential instantiation |
|<table><tr><td></td><td style="font-style:italic">P(c) for some element c</td></tr><tr><td style="font-weight:bold;background: rgba(255,255,255,.1)">∴</td><td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">∃xP(x)</td></tr></table>| Existential generalization |

### Universal Instantiation

This is the rule used to conclude that *P(c)* is true, where *c* is a particular member of the domain, given the premise *∀xP(x)*.

For example, take this statement:

"All women are wise"
"Lisa is wise"

Here we can use the rule of Universal Instantiation to conclude that because all women are wise (*∀xP(x)*), any "instantiation" will be true, in this case Lisa (*c*).

### Universal generalization

This is the rule used to conclude that *∀xP(x)* is true, given the premise that *P(c)* is true **for all ements *c* in the domain**.

In order to use this rule, we must take an arbitrary, **not** specific, element *c* from the domain and show that *P(c)* is true.

### Existential instantiation

This rule is used to conclude that there is an element *c* in the domain for which *P(c)* is true if we know that *∃xP(x)* is true.

We must not select an arbitrary value of *c* here, but rather it must be a *c* for which *P(c)* is true.

### Existential generalization

This rule is used to conclude that *∃xP(x)* is true when a particular element *c* with *P(c)* true is known. So, if we know one element *c* in the domain for which *P(c)* is true, then we know that *∃xP(x)* is true.

## Combining Rules of Inference for Propositions and Quantified Statements

### Universal Modus Ponens

This is the combination of *Universal instantiation* and *modus ponens*.

It tells us that if *∀x(P(x) → Q(x))* is true, and if *P(a)* is true for a particular element *a* in the domain of the universal quantifier, then *Q(a)* must also be true!

Here's the full rule:

<table>
	<tr>
		<td></td>
		<td style="font-style:italic">
		∀x(P(x) → Q(x))
		</td>
	</tr>
	<tr>
		<td></td>
		<td style="font-style:italic">
		P(a), where a is a particular element in the domain
		</td>
	</tr>
	<tr>
		<td style="font-weight:bold;background: rgba(255,255,255,.1)">
		∴
		</td>
		<td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">
		Q(a)
		</td>
	</tr>
</table>

So for example we could use it to form an argument for the following:

"All students of the class who passed the exam also read the book"
"Lisa passed the exam"
"Therefore, Lisa read the book"

### Universal Modus Tollens

This one combines universal instantiation and modus tollens.

It tells us that if *∀x(P(x) → Q(x))* is true, and if *¬Q(a)* for a particular element *a* in the domain of the universal quantifier, then *¬P(a)* must also hold!

Here's the full rule:

<table>
	<tr>
		<td></td>
		<td style="font-style:italic">
		∀x(P(x) → Q(x))
		</td>
	</tr>
	<tr>
		<td></td>
		<td style="font-style:italic">
		¬Q(a), where a is a particular element in the domain
		</td>
	</tr>
	<tr>
		<td style="font-weight:bold;background: rgba(255,255,255,.1)">
		∴
		</td>
		<td style="font-style:italic;font-weight: bold;background: rgba(255,255,255,.1)">
		¬P(a)
		</td>
	</tr>
</table>

## Proof terminology

### Theorem (again)

A Theorem is a statement that can be shown to be true.
It may also be called a *fact* or a *result*.

### Propositions (again)

Less important theorems are sometimes called propositions.

### Axioms

An Axiom is a statement we assume to be true. Something "we know".

It can also be called a *postulate*.

### Lemmas

A *lemma* is a less important theorem that is helpful in the proof of other results.

## How Theorems are stated

Many mathematical theorems require a universal quantifier, but the standard convention in mathematics is to omit it.

## Direct Proofs

A *direct proof* of a conditional statement *p → q* is constructed **when the first step is the assumption that *p* is true**.

It shows that a conditional statement *p → q* is true by showing that if *p* is true, then *q* must also be true. So, the combination *p* true and *q* false never occurs.

The first step of such proof is the assumption that *p* is true. Then, the following steps are constructed using rules of inference. The final step shows that *q* must also be true.

Simply:

```text
If p, then q
Assume p
Show that q
```

## Indirect proofs

Proofs that *do not* start with the premises and end with the conclusion are called **indirect proofs**.

## Proof by Contraposition

This is a type of indirect proof.
It makes use of the fact that the conditional statement *p → q* is equivalent to its contrapositive *¬q → ¬p*.

Here, we take *¬q* as a premise and show that *¬p* must follow.

This can succeed when we cannot easily find a direct proof.

We assume that the conclusion of the conditional statement is false and work from there.

Simply:

```text
If p, then q
Assume ¬q
Show that ¬p
```

This is fine since contraposition is equivalent to the original logical statement.

## Proof by Contradiction

If we can find a contradiction *q*, it will help us prove that *p* is true.

Because the statement *r ∧ ¬r* is a contradiction, we can prove that *p* is true if we can show that *¬p → (r ∧ ¬r)* is true for some proposition *r*.

This is called a *Proof by Contradiction*.

```text
We want to prove p
Assume ¬p
Find some contradiction q ∧ ¬q
Claim ¬¬p which is the same as p
```

## Simple proof strategy

1. First see if a direct proof looks promising. Begin by expanding the definitions in the hypotheses.
2. If a direct proof doesn't see, to go anywhere, try again with a proof by contraposition.
3. Otherwise, go for proof by contradiction.

## Proofs of equivalence

We can also prove a theorem that is a biconditional statement (*p ↔ q*), we simply show that *p → q* and *q → p* are both true.

## Proof by counterexample

We can prove that a statement of the form *∀xP(x)* is false by finding a counterexample - that is, an example *x* for which *P(x)* is false.

## Proof by cases

Sometimes we cannot prove a theorem using a single argument that holds for all possible cases.

Instead, we can prove each of the *n* statements individually.

A proof by cases must cover all possible cases that arise in a theorem.

For example, sometimes to prove that a conditional statement *p → q* is true, it is convenient to use a disjunction:

*p<sub>1</sub> ∨ p<sub>2</sub> ∨ ... ∨ p<sub>n</sub>* instead of *p* as the hypothesis of the conditional statement, where *p* and *p<sub>1</sub> ∨ p<sub>2</sub> ∨ ... ∨ p<sub>n</sub>* are equivalent.

## Exhaustive proof

Exhaustive proofs are ones that proceed by exhausting all possibilities. These are special types of proofs by cases where each case involves checking a single example.

## Without Loss Of Generality (WLOG)

When this phrase is used in a proof, we assert that by proving one case of a theorem, no additional argument is required to prove other specified cases. Other cases follow by making straightforward changes to the argument, or by filling in some straightforward initial step.

## Existence Proofs

A proof of a proposition of the form *∃xP(x)* is called an *existence proof*.

### Constructive existence proofs

Here, you find an element *a* called a *witness* such that *P(a)* is true.

### Nonconstructive existence proofs

Here you do not find an element *a* such that *P(a)* is true, but rather prove that *∃xP(x)* is true in some other way.

We could do this with proof by contradiction and show that the negation of the existential quantification implies a contradiction.

## Uniqueness Proofs

Here we prove that there is exactly one element with a particular property. We need to show that an element with this property exists and that no other element has this property.

There are two parts to a *uniqueness proof*. They are:

- **Existence**: We show that an element *x* with the desired property exists.
- **Uniqueness**: We show that if y != x, then y does not have the desired property

## Full Proof Strategy

1. First, replace terms by their definitions
2. Carefully analyze what the hypotheses and the conclusion mean.
3. Attempt to prove the result using one of the available methods of proof.
	- Generally, if the statement is a conditional statement, start with trying a direct proof.
	- If that fails, try an indirect proof.
	- If that fails, try a proof by contradiction.