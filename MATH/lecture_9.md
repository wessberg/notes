# Lecture 9

> Discrete Mathematics and Its Applications, Chapter 9.1, 9.3, 9.5, 9.6 (excluding "Lattices")

## Relations and Their Properties

### Binary relations

We can express a relationship between elements of two sets by using ordered pairs made up of two related elements. This is called *binary relations*.

Let *A* and *B* be sets. A *binary relation* from *A* to *B* is a subset of *A x B*.

In other words, a binary relation from *A* to *B* is a set *R* of ordered pairs where the first element of each ordered pair comes from *A* and the second element comes from *B*.

We use the notation *a R b* to denote that *(a, b) ∈ R*

When *(a, b)* belongs to *R*, *a* is said to be **related to** *b* by *R*.

## Functions as Relations

A function from a set *A* to a set *B* assigns exactly of element of *B* to each element of *A*.

Thus, the graph of *f* is a subset of *A x B*. It is a relation from *A* to *B*. And, every element of *A* is the first element of exactly one ordered pair of the graph.

## Relations on a Set

A relation on a set *A* is a relation from *A* to *A*.
In other words, a relation on a set *A* is a subset of *A x A*.

## Properties of Relations

### Reflexive

A relation *R* on a set *A* is called *reflexive* if *(a, a) ∈ R* for every element *a ∈ A*.

For example, consider the following relation on *{1, 2, 3, 4}*:

*R = {(1,1), (1, 2), (1, 4), (2, 1), (2, 2), (3, 3), (4, 1), (4, 4)}*.

This one **is** reflexive, because it contains all pairs of the form *(a, a)*, namely *(1,1), (2,2), (3,3)* and *(4,4)*.

However this one:

*R = {(1,1), (1,2), (2,1)}*.

is **not**, since it does **not** contain all pairs of the form *(a, a)*. It misses *(2,2), (3,3)* and *(4,4)*.

### Symmetric

A relation *R* on a set *A* is called *symmetric* if *(b, a) ∈ R* whenever *(a, b) ∈ R* for all *a, b ∈ A*.

In other words, a relation is symmetric if and only if *a* is related to *b* implies that *b* is related to *a*.

For example:

*R = {(1,1), (1,2), (2,1)}*.

This relation is symmetric because both the pairs *(1, 2)* and *(2, 1)* is represented.

### Antisymmetric

A relation *R* is *antisymmetric* if given a set *A*, for all *a, b ∈ A, if (a,b) ∈ R and (b,a) ∈ R, then a = b*.

In other words, if both the combination *(a,b)* and *(b,a)* appears in a relation *R*, then *a* and *b* must be identical for it to be antisymmetric.

For example:

*R = {(1,1)}*.

This one is antisymmetric. It's also symmetric.

### Transitive

A relation *R* on a set *A* is called *transitive* if whenever *(a, b) ∈ R* and *(b, c) ∈ R*, then *(a, c) ∈ R* for all *a, b, c ∈ A*.

## Combining Relations

Relations can be combined just fine.

Given two relations:

*R<sub>1</sub> = {(1,1), (2,2), (3,3)}* and *R<sub>2</sub> = {(1,1), (1,2), (1,3), (1,4)}*

These can be combined:

*R<sub>1</sub> ∪ R<sub>2</sub> = {(1,1), (2,2), (3,3), (1,2), (1,3), (1,4)}*
*R<sub>1</sub> ∩ R<sub>2</sub> = {(1,1)}*
*R<sub>1</sub> - R<sub>2</sub> = {(2,2), (3,3)}*
*R<sub>2</sub> - R<sub>1</sub> = {(1,2), (1,3), (1,4)}*

### Composite

Let *R* be a relation from a set *A* to a set *B* and *S* a relation from *B* to a set *C*.

The *composite* of *R* and *S* is the relation consisting of ordered pairs *(a, c)* where *a ∈ A*, *c ∈ C*, and for which there exists an element *b ∈ B* such that *(a, b) ∈ R* and *(b, c) ∈ S*.

We denote the composite of *R* and *S* by *S ◦ R*.

**To compute the composite of two relations, we find elements that are the second element of ordered pairs in the first relation and the first element of ordered pairs in the second relation. For all of them, we take the first element of the first relation and the second element of the second relation to make up a new pair**.

For example:

*R = {(1, 4)}*.
*S = {(4, 2)}*

*R ◦ S = {(1, 2)}* since the first ordered pair of *R* was matched with the first ordered pair of *S*. We then took the first element of the first ordered pair of *R* and the second element of the first ordered pair of *S* to form our new ordered pair *(1, 2)*.

## Representing Relations

We can represent a relation using Matrices (tables) or with directed graphs.

## Equivalence Relations

A relation on a set *A* is called an *equivalence relation* if it is reflexive, symmetric and transitive.

Two elements *a* and *b* that are related by an equivalence relation are called *equivalent*. We often use the notation *a ∼ b* to denote that *a* and *b* are equivalent elements with respect to a particular equivalence relation.

## Equivalence Classes

Let *R* be an equivalence relation on a set *A*. The set of all elements that are related to an element *a* of *A* is called the *equivalence class* of *a*. The equivalence class of *a* with respect to *R* is denoted by *[a]<sub>R</sub>*. When only one relation is under consideration we can just write *[a]* for this equivalence class.

### Representatives of equivalence classes

if *b ∈ [a]<sub>R</sub>*, then *b* is called a **representative** of this equivalence class.

## Partial Orderings

A relation *R* on a set *S* is called a *partial ordering* or *partial order* if it is reflexive, **antisymmetric**, and transitive.

A set *S* together with a partial oredering R* is called a *partially ordered set* or *poset* for short. It is denoted by *(S, R)*. Members of *S* are called *elements* of the poset.

The rest was simply too TLDR; to me.