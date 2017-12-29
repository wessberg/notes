# Lecture 4

> Discrete Mathematics and Its Applications, Chapter 24.1, 4.2, 4.3, skim 4.4, skim 4.5

## Divisibility and Modular Arithmetic

Division of an integer by a positive integer produces a quotient and a remainder. Working with these remainders leads to modular arithmetic!

### Division

When dividing an integer by a second nonzero integer, the quotient may not be an integer.

Here it is: *12 / 3 = 4*, but here it isn't *11 / 4 = 2.75*.

Formally, if *a* and *b* are integers with *a != 0*, we say that *a divides b* if there is an integer *c* such that *b = a * c* or if *b/a* is an integer.

Then, *a* is a *factor* or a *divisor* of *b* and that *b* is a multiple of *a*.

Notation: *a|b* denotes that *a* divides *b*.
Otherwise, if *a* does not divide *b*, we write *a ∤ b*.

Note that when we read *a|b*, we actually divide it like this: *b/a*.

We can also express this visibility with: *∃c(ac = b)*.

#### Basic properties of divisibility of integers

Where *a "= 0*:

- If *a|b* and *a|c* then *a | (b + c)*
- If *a|b*, then *a|bc* for all integers *c*
- If *a|b* and *b|c* then *a|c*

### The Division Algorithm

Let *a* be an integer and *d* a positive integer. Then there are unique integers *q* and *r* with *0 <= r < d* such that *a = dq + r*.

- *d* is called the divisor
- *a* is called the *dividend*
- *q* is called the *quotient*
- *r* is called the *remainder*

We express the quotient and remainder:
*q = a **div** d*, *r = a **mod** d*.

For example:

If we are to decide what the quotient and remainder is when 101 is divided by 11:

1. First, see how much you can multiply 11 with before 11 > 101. You can do this by `Math.floor(101/11)`. The result is 2. Now, this is the quotient.
2. The remainder is then the amount of remaining integers before reaching 101 (in this case, 2).

So: *101 = 11 * 9 + 2*.

## Modular Arithmetic

Often we only care about the remainder of an integer when it is divided by some specified positive integer!

Example: When we want to know what time it will be 50 hours from now on a 24-hour clock, we care only about the remainder when (50 + current_hour) is divided by 24.

So the algorithm would simply by `(50 + current_hour) % 24`.

### Notation when two integers have the same remainder when they are divided by the positive integer *m*

If *a* and *b* are integers and *m* is a positive integer, then *a* is *congruent to b modulo m* if *m* divides *a - b*.

Notation: *a ≡ b (mod m)*.

In other words, *a ≡ b (mod m)* if and only if *a **mod** m = b **mod** m*.

So, basically, *a* is congruent to *b modulo m* if they have the same remainder from the division with *m*.

First, take *a - b* and then divide that by *m*. If you get an integer, then *m* divides *a - b* and we're golden. But you could also just do *a mod m* and *b mod m* and compare the results.

We could also just say *(a **mod** m) = (b **mod** m)*.

### Arithmetic Modulo *m*

This is basically just "syntactic sugar".

This:

*a +<sub>m</sub> b* is equal to *(a + b) **mod** m*.

And this:

*a *<sub>m</sub> b* is equal to *(a * b) **mod** m*.

For example:

*7 +<sub>11</sub> 9 = (7 + 9) **mod** 11 = 16 **mod** 11 = 5*.

## Integer Representations and Algorithms

Numbers are normally represented in *decimal* (base 10), but there are plenty of other representations such as base 2 (binary), octal (base 8) and hexadecimal (base 16).

### Base *b* expansion

The base *b* expansion of *n* is denoted by *n<sub>b</sub>*. For example, *(245)<sub>8</sub>*.

This is a way to take something that is in base *b* and convert to another base (if subscript is omitted, it defaults to decimal). What you do is that you multiply each character by the base, *b*, raised to the power of its 0-indexed position in the string from right to left and add them together.

For example, in *(245)<sub>8</sub>*, *2* is at position *2*, *4* is at position *1* and *5* is at position *0*.

Then it becomes *2 * 8<sup>2</sup> + 4 * 8<sup>1</sup> + 5 * 8<sup>0</sup> = 165*.

Another example is the decimal expansion of a binary expresion: *(101011111)<sub>2</sub>*. This is easy enough. Again, for each character, raise the base (in this case, *2*) to the power ofthe character's 0-indexed position in the string from right to left. Multiply by the character itself. Add the results together:

*1 * 2<sup>8</sup> + 0 * 2<sup>7</sup> + 1 * 2<sup>6</sup> + 0 * 2<sup>5</sup> + 1 * 2<sup>4</sup> + 1 * 2<sup>3</sup> + 1 * 2<sup>2</sup> + 1 * 2<sup>1</sup> + 1 * 2<sup>0</sup> = 351*.

## Primes and Greatest Common Divisors

### Prime Numbers

*Every* integer greater then 1 is divisible by at least two integers - 1 and by itself! They may have many more.

A *prime* is an integer greater than 1 **that is divisible by no positive integers other than 1 and itself**.

Formally, an integer *p* greater than 1 is called *prime if and only if the only positive factors of *p* are 1 and *p*.

#### Composites

Any integer greater than 1 which isn't a prime (e.g. is divisble by more than 1 and itself) is called a **composite**.

**Every integer greater than 1 can be written uniquely as a prime or as the product of two or more primes where the prime factors are written in order of nondecreasing size!**

### Showing that a given integer is prime with Trial Division

If *n* is a composite integer, then *n* has a prime divisor less than or equal to *√n*.

So, if an integer is not divisible by any *prime* less than or equal to its square root, it is a *prime*!

A brute-force algorithm called *trial division* can be used here.

With Trial Division, we divide *n* by all primes not exceeding *√n* and conclude that *n* is prime if it is not divisible by any of these primes!

For example:

To show that *101* is a prime with Trial Division, we first take all primes not exceeding *√101*.
These are *2, 3, 5 and 7*.

101 is not divisible by any of these - since the quotient of 101 and each of these integers is not an integer. Thus, 101 *is* prime!

### Finding the prime factorization of any integer

To find the prime factorization of *n*, perform division of *n* by successive primes, beginning with 2. When you reach a prime that divides *n*, assign the result of the division to *n* and continue trying to find a prime that divides *n* from and including the current prime. Continue doing so until you cannot go further.

### How to find all primes not exceeding a specified positive integer

1. First, all integers that are divisible by 2 other than 2 are deleted.
2. Now, take the first remaining integer (that isn't 2). In the first step, this will be 3. Now, remove all those integers that are divisible by 3 other than 3.
3. Now, take the first remaining integer (that isn't 2 or 3). This will be 5. Continue until the first remaining integer (that isn't 2, 3, or 5) is greater than the specified positive integer (the upper limit)

**There are infinitely many primes!** There is always a prime larger than whatever prime we are looking at.

## Greatest Common Divisiors and Least Common Multiples

### Greatest Common Divisor

The largest integer that divides both of two integers are called the *greatest common divisor*o fthese integers.

Formally:

Let *a* and *b* be integers, not both zero. The largest integer *d* such that *d|a* and *d|b* is called the *greatest common divisor* of *a* and *b*.

**This is denoted by *gcd(a, b)***.

For example, the greatest common divisor of 24 and 36 is *gcd(24, 36) = 12*.

We can perform Trial Division on both until we arrive at the highest number that divides both.

Two numbers always have a common divisor - 1. So if two integers, for example 17 and 22 have no other common divisors, 1 is fine as an answer.

### Finding the *gcd* with prime factorizations

Suppose that the prime factorizations of the positive integers *a* and *b* are:

*a = p<sub>1</sub><sup>a<sub>1</sub></sup> p<sub>2</sub><sup>a<sub>2</sub></sup> ... p<sub>n</sub><sup>a<sub>n</sub></sup>, b = p<sub>1</sub><sup>b<sub>1</sub></sup> p<sub>2</sub><sup>b<sub>2</sub></sup> ... p<sub>n</sub><sup>a<sub>n</sub></sup>*

Where each exponent is a nonnegative integer, and where all primes occuring in the prime factorization of either *a* or *b* are included in both factorizations, then *gcd(a,b)* is given by:

*gcd(a,b) = p<sub>1</sub><sup>min(a<sub>1</sub>, b<sub>1</sub>)</sup> p<sub>2</sub><sup>min(a<sub>2</sub>, b<sub>2</sub>)</sup> ... p<sub>n</sub><sup>min(a<sub>n</sub>, b<sub>n</sub>)</sup>*

Where *min(x, y)* represents the minimum of the two numbers *x* and *y*.

### Least Common Multiple

The *least common multiple* of two positive integers *a* and *b* is the smallest positive integer **that is disible by both *a* and *b***.

## The Euclidean Algorithm

It is quite inefficient to compute the gcd with prime factorizations.

The Euclidean algorithm is much more efficient for this.

For two numbers 91, 287, here's how to compute *gcd(91, 287)*

1. **Take the remainder of dividing the largest of the two integers with the smaller one!**. In this case, *287 **mod** 91 = 14*.
2. Now, the problem has been reduced to finding *gcd(91,14)* where *91* is the smallest of the current integers and *14* was the remainder from the division.
3. Proceed continuously this way. Take the remainder of dividing the largest (91) integer with the smaller one (14) (which is 7). Call *gcd(14, 7)* - 14 is the smallest of the current two integers, and 7 is the remainder.
4. Continue until one of the integers is zero! When it is, we have our answer

In code:

```javascript
function gcd (num1, num2, candidate = 1) {
	if (num1 <= 0 || num2 <= 0 || candidate <= 0) return candidate;

	const smallest = Math.min(num1, num2);
	const largest = Math.max(num1, num2);
	const remainder = largest % smallest;

	if (remainder <= 0) return candidate;

	return gcd(smallest, remainder, remainder);
}
```