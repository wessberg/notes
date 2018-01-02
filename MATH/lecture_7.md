# Lecture 7

> Discrete Mathematics and Its Applications, Chapter 6.1, 6.2, 6.3

## Basic Counting Principles

Two important principles:

- The **product rule**
- The **sum rule**

### The Product Rule

If we can break a procedure down into a sequence of two tasks, and if there are *n<sub>1</sub>* ways to do the first task, and **for each of these ways** of doing the first, task there are *n<sub>2</sub>* ways to do the second task, then there are *n<sub>1</sub>n<sub>2</sub>* ways to do the procedure.

For example:

*A new company with just two employees, Sanchez and Patel, rents a floor of a building with 12 offices. How many ways are there to assign different offices to these two employees?*

Solution:

1. First you find the amount of combinations of offices for the first employee, Sanchez. That is 12.

2. Now there are 11 free office spaces. Now, you find the amount of combinations of offices for the second employee. That is 11.

3. Now you multiply the two results: *12 * 11 = 132*. There are 132 ways to assign offices to these two employees.

Another example:

*The chairs of an auditorium are to be labeled with an uppercase English letter followed by a positive integer not exceeding 100. What is the largest number of chairs that can be labeled differently?*

Solution:

1. There are 26 letters in the english alphabet. There are 100 positive integers. The result is *26 * 100 = 2600* different ways.

Another example:

*There are 32 microcomputers in a computer center. Each microcomputer has 24 ports. How many different ports to a microcomputer in the center are there?*

Solution:

There are *32 * 24 = 768* ports

Another example:

*How many different bit strings of length seven are there?*

Solution:

There are |{0, 1}| = 2 possible values on each position in the bit string. There are 7 positions, each of which can beither 0 or 1. As such, there are 2<sup>7</sup> = 128 different bit strings of length 7.

### The Sum Rule

It states that if a task can done either in one of *n<sub>1</sub>* ways or in one of *n<sub>2</sub>* ways, where none of the set of *n<sub>1</sub>* ways is the same as any of the set of *n<sub>2</sub>* ways, then there are *n<sub>1</sub> + n<sub>2</sub>* ways to do the task.

For example:

*Suppose that either a member of the mathematics faculty or a student who is a mathematics major is chosen as a representative to a university committee. How many different choices are there for this representative if there are 37 members of the mathematics faculty and 83 mathematics majors and no one is both a faculty member and a student?*

Solution:

By the Sum rule, there are *37 + 83 = 120*.

Another example:

*A student can choose a computer project from one of three lists. The three lists contain 23, 15, and 19 possible projects, respectively. No project is on more than one list. How many possible projects are there to choose from?*

Solution:

Well, there are *23 + 15 + 19 = 57* ways.

## Combining the Product and Sum rule

Sometimes we can combine the two rules when counting.

For example:

*In a version of the computer language BASIC, the name of a variable is a string of one or two alphanumeric characters, where uppercase and lowercase letters are not distinguished. (An alphanumeric character is either one of the 26 English letters or one of the 10 digits.) Moreover, a variable name must begin with a letter and must be different from the five strings of two characters that are reserved for programming use. How many different variable names are there in this version of BASIC?*

Solution:

First, there is 26.
Then there is 26 * (26 + 10) - 5 = 931.
This amounts to *26 + 931 = 957*

## The Subtraction Rule

If a task can be done in either *n<sub>1</sub>* ways or *n<sub>2</sub>* ways, then the number of ways to the task is *n<sub>1</sub> + n<sub>2</sub>* **minus the number of ways to do the task that are common to the two different ways**.

For example:

*A computer company receives 350 applications from computer graduates for a job planning a line of new Web servers. Suppose that 220 of these applicants majored in computer science, 147 majored in business, and 51 majored both in computer science and in business. How many of these applicants majored neither in computer science nor in business?*

Solution:

*220 + 147 - 51 = 316*.

## The Division Rule

It states that there are *n / d* ways to do a task if it can be done using a procedure that can be carried out in *n* ways, and for every way *w*, exactly *d* of the *n* ways correspond to way *w*.

## The Pigeonhole Principle

States that if *k* is a positive integer and *k + 1* or more objects are placed into *k* boxes, then there is at least one box containing two or more of the objects.

## The Generalized Pigeonhole Principle

States that if *N* objects are placed into *k* boxes, then there is at least one box containing at least *Math.ceil(N/k)* objects.

## Permutations and Combinations

### Permutations

A *permutation* of a set of distinct objects is an ordered arrangement of these objects.

We say *how many ways can we fit n elements into k items*. For example, *how many ways can we seat 6 people (n) into 4 chairs (k)* The formula is:

*n! / (n-k)!*.

This is also called the Binomial-coefficient.

So, that's how many *permutations* there are.

But, if the question then is *How many ways can we choose k elements of a pool of n*, for example *How many ways can we choose 4 people (k) from a group of 6(n) people*, the formula becomes:

*n! / (k! * (n-k)!)*.

That's how many *combinations* there are.

**Permutations are for lists (order matters) and combinations are for groups (order doesn't matter)**.

Now, whatif we want to say **

#### *r*-permutation

An ordered arrangement of *r* elements of a set is called an *r*-permutation.

For example,

For the set *{1, 2, 3}*, an ordered arrangement *3, 1, 2* is a **permutation**.

And, the ordered arrangement *3, 2* is a 2-permutation of S (since we only use two elements of the set).

### Combinations

#### *r*-combinations

An *r*-combination of elements of a set is an unordered selection of *r* elements from the set.

**In other words, it is simply a subset of the set with *r* elements**.

For example, given the set *{1, 2, 3, 4}*, *{1, 3, 4}* is a perfectly fine *3-combination* of the original set.

#### Counting the amount of *r*-combinations of a set with *n* elements where *n* is a nonnegative integer and *r* is an integer with *0 <= r <= n*

Yes: *n! / r!(n -r)!*