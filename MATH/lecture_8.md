# Lecture 8

> Discrete Mathematics and Its Applications, Chapter 7.1, 7.2 (until "The birthday problem"), 7.3 (until "Bayesian Spam Filters"), 7.4 (Exclusing sections "Average-Case Computational Complexity", "The Geometric Distribution", and "Chebyshev's Inequality", and in the section "Variance", only until Theorem 7)

## Finite Probability

### Experiment

An *experiment* is a procedure that yields one of a given set of possible outcomes.

### Sample space

The *sample space* of an experiment is the set of possible outcomes.

### Event

An *event* is a subset of the sample space.

### Probability of an event

If *S* is a finite nonempty sample space **of equally likely** outcomes, and *E* is an event, that is, a subset of *S*, then the *probability* of *E* is *p(e) = |E| / |S|*.

So, the probability of an event is **between 0 and 1**.

For example:

*An urn contains four blue balls and five red balls. What is the probability that a ball chosen at random from the urn is blue?*

Solution:

There are 9 balls in total. 4 of them are blue.
So, the probability is 4/9.

Another example:

*What is the probability that the numbers 11, 4, 17, 39, and 23 are drawn in that order from a bin containing 50 balls labeled with the numbers 1, 2,..., 50 if (a) the ball selected is not returned to the bin before the next ball is selected and (b) the ball selected is returned to the bin before the next ball is selected?*

Solution:

By the product rule, there are *50 * 49 * 48 * 47 * 46 = 254,251,200* ways to select the balls. And, the probability that the appear in the order 11, 4, 17, 39, 23 is *1 / 254,251,200 = 0.000000003933118*, an insanely small number.

## Probabilities of Complements and Unions of Events

We can use counting techniques to find the probability of events **derived from other events**.

Let *E* be an event in sample space *S*. The probability of the *complementary* event of *E*, denoted, *<span style="text-decoration: overline">E</span>* is *P(<span style="text-decoration: overline">E</span>) = 1 - p(E)*

Often it is more practial to find the probability of the *complement* of an event than the event itself.

For example:

*A sequence of 10 bits is randomly generated. What is the probability that at least one of these bits is 0?*

Solution:

Let *E* be the event that at least one of the 10 bits is 0. Then *<span style="text-decoration: overline">E</span>* is the event that all the bits are *1*s. Because the sample space *S* is the set of all bit strings of length 10, it follows that:

*p(E) = 1 - p(<span style="text-decoration: overline">E</span>) = 1 - |<span style="text-decoration: overline">E</span>| / |S| = 1 - (1 / 2<sup>10</sup>) = 1 - 1 / 1024 = 1023/1024*

### Probabily Theory

The probability of an event was defined as *p(E) = |E| / |S|* under the assumption that **all outcomes are equally likely**. But that is often not the case!

## Conditional Probability

When flipping a coin, does knowing that the first flip comes up heads change the probability that heads comes up three times?

If not, these two events are called *independent*.

Let *E* and *F* be events with *p(F) > 0*. The *conditional probability* of *E* given *F*, denoted by *p(E | F)*, is defined as:

*p(E | F) = p(E ∩ F ) / p(F)*.

### Assiging probabilities

Let *S* be the sample space of an experiment with a finite or countable number of outcomes. We assign a probability *p(s)* to each outcome *s*. We require that:

- The probability is in the range *[0, 1]* for each *s ∈ S*.
- The sum of all probabilities is 1. This also means that it is a certainty that one of these outcomes occurs.

#### Probability distribution

The function *p* from the set of all outcomes of the sample space *S* is called a *probability distribution*.

For example:

*What probabilities should we assign to the outcomes H (heads) and T (tails) when a fair coin is flipped? What probabilities should be assigned to these outcomes when the coin is biased so that heads comes up twice as often as tails?*

Solution:

In terms of a fair coin, the probabilities should be *p(H) = 1 / 2* and *P(T) = 1 / 2*.

In terms of the other one, we have that *p(H) = 2p(T)*.

We conclude that *p(T) = 1 / 3* and *p(H) = 2 / 3*

#### Uniform distribution

Suppose that *S* is a set with *n* elements. The *uniform distribution* assigns the probability *1 / n* to each element of *S*.

### Refined definition of Probability of an event

The *probability* of the event *E* is the sum of the probabilities of the outcomes in *E*:

*p(E) = ∑<sub>s ∈ E</sub> p(s)*

So, if there are *n* outcomes in the event *E*, we sum them up.

For example:

*Suppose that a die is biased (or loaded) so that 3 appears twice as often as each other number but that the other five outcomes are equally likely. What is the probability that an odd number appears when we roll this die?*

Solution:

There are 6 possible outcomes.
We have that:
*p(1) = p(2) = p(4) = p(5) = p(6) = 1 / 7*
and
*p(3) = 2 / 7*

Now, the odd numbers are *{1, 3, 5}*. We want to find the probability that any of these occur when we roll the die.

So its *p(1) + p(3) + p(5) = 1 / 7 + 2 / 7 + 1 / 7 = 4 / 7*

## Independence

If two events *E* and *F* are independent, it does not alter the propability of *E* that *F* has occured. In otherwords, *p(E | F) = p(E)*.

Formally, two events *E* and *F* are *independent* if and only if *p(E ∩ F) = p(E)p(F)*

## Pairwise and Mutual Independence

We can also define the independence of more than two events.

The events *E<sub>1</sub>, E<sub>2</sub>, ..., E<sub>n</sub>* are *pairwise independent* if and only if *p(E<sub>i</sub> ∩ E<sub>j</sub>) = p(E<sub>1</sub>)p(E<sub>j</sub>)* for all pairs of integers *i* and *j* with *1 <= i < j <= n*.

## Bernoulli Trials

A Bernoulli Trial is a performance of an experiment with two possible outcomes. A possible outcome is called a *success* or a *failure*.

## Random Variables

A *random variable* **is a function** from the sample space of an experiment to the set of real numbers. That is, a random variable assigns a real number to each possible outcome.

**A Random variable is a function. It is neither a variable nor random**.

So, the name is very misleading.

For example:

*Suppose that a coin is flipped three times. Let X(t) be the random variable that equals the number of heads that appear when t is the outcome. Then X(t) takes on the following values:*

*X(HHH) = 3*
*X(HHT) = X(HTH) = X(THH) = 2*
*X(TTH) = X(THT) = X(HTT) = 1*
*X(TTT) = 0*

## Bayes' Theorem

We can use Bayes' Theorem to derive a more realistic estimate that a particular event occurs based on extra information available.

Suppose we know *p(F)*, the probability that an event *F* occurs, but we have *knowledge* that an event *E* occurs. Then the conditional probability that *F* occurs given that *E* occurs, *p(F|E)* is a more reaslitic estimate than *p(F)* that *F* occurs.

We can find *p(F | E)* when we know *p(F)*, *p(E|F)* and *p(E|<span style="text-decoration: overline">F</span>)*

## Expected Value and Variance

### Expected value

We often expect some outcome of an experiment. For example, if the experiment is: "How many heads are expected to appear when a coin is flipped 100 times?", we expect the answer to be *50"*.

Formally, the *expected value* (which is also called the *expectation* or *mean*), of the random variable *X* on the sample space *S* is equal to:

*E(X) = Σ<sub>s ∈ S</sub>p(s)X(s)*

The *deviation* of *X* at *s ∈ S* is *X(s) - E(X)*, the difference between the value of *X* and the mean of *X*.

## Variance

The expected value of a random variable tells us its average value, but nothing about how widely its values are distributed.

The *variance* helps us characterize how widely a random variable is distributed about its expected value.

The *variance* of *X* is denoted by *V(X)* and is:

*V(X) = Σ<sub>s ∈ S</sub>(X(s) - E(X))<sup>2</sup>p(s)*

In words, *V(X)* is the weighted average of the square of the deviation of *X*.