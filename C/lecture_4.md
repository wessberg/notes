# Lecture 4

> CS:APP, Chapter 2.3

## From the slides

### Slides: Memory

From the slides:

Memory is an array of bytes.
Each byte in memory has an address.
Each address is encoded on 8 bytes on a 64-bit system.

### Slides: Arrays vs pointers

Arrays have a size, pointers do not!
You have to be careful since the notation for arrays and pointers can be used interchangably, for example:

`a[i] = *(a+i)`.

**Pointers must always be initialized!**

## One's complement

With this method, if we have a value and we want to make it negative, we simply flip the bits.

For example, if we have the decimal *1* (which is *001* in binary),
with One's Complement, we would simply flip the bits if we want *-1* (which is *110* in binary).

Here's a few examples of decimal numbers and their binary representation using One's complement in a 3-bit system (a system where the word size *w* is 3.

|   binary   |   decimal   |
|------------|-------------|
|    000     |      0      |
|    001     |      1      |
|    010     |      2      |
|    011     |      3      |
|    100     |     -3      |
|    101     |     -2      |
|    110     |     -1      |
|    111     |     -0      |

Notice how 0 is represented twice - once as 0 (for 000) and once as -0 (for 111).

Two's Complement solves this:

### Two's Complement

With this method, we:

1. first flip the bits first - just like with One's Complement.
2. We add 1.

That's it!

So, if I have a 0 (000) in binary, to get the negative value, I would:

1. Flip the bits. Now I have 111, like with One's Complement.
2. Now, I add one. That is, 111 + 001 = 1000 (in binary).

This is a 4-bit number, but all values so far has been 3-bit numbers, so we can simply ignore the leading bit (1). Ultimately we end up with 000, effectively having only 1 representation of zero - 000.

Let's see how it goes for the remaining values (with Two's Complement) in a 3-bit system (a system where the word size *w* is 3:

|   binary   |   decimal   |
|------------|-------------|
|    000     |      0      |
|    001     |      1      |
|    010     |      2      |
|    011     |      3      |
|    100     |     -4      |
|    101     |     -3      |
|    110     |     -2      |
|    111     |     -1      |

Notice that 100 is equal to -4. That will always be equal to the smallest negative value in our system (depending on the word size *w*).

#### Observations with Two's-Complement

- -1 will *always* be 111 (or more 1's, depending on the word size)
- The smallest negative number, will *always* be represented by 100 (or more 0's, depending on the word size)
- The largest positive number will *always* be represented 011 (or more 1's, depending on the word size)

### Addition with Two's Complement

One of the major strong points of Two's Complement is that it makes things like addition really easy in that we can simply add the bits together.

Say we want to add 2 + 1, which is 010 + 001 = 011. 011 is 3, so it works.

### Subtraction

Now, for subtraction, say we want to do 3 - 1.

To do this, instead of subtracting, **we add the negative representation of whatever is to be extracted (in this case 1)**.

So, in 3 - 1, the negative representation of what is to be extracted is -1 which is 111 in binary, so way say:

011 + 111 = 1010. We ignore the leading *1* since this is a 3-bit system and end up with 010 which is 2 in decimal. This is the right result!

Now, let's try something where we end up with a negative result, like 2 - 4.

Again, we know that 2 is 010 in binary. -4 is 100 in binary. So, we end up with:

010 + 100 = 110 which is -2 in binary and again the right result.

## Overflow

Now, if the word size *w* of our system is 3, with Two's Complement, the full range of supported integers is [-4, -3, -2, -1, 0, 1, 2, 3]. As you can see, that is 8 values which is exactly as expected since *2<sup>3</sup>* is 8.

But then, let's say we do: 3 + 2 = 5. This is easy to do in decimal, but this in fact *overflows* our system (which supports up to the positive integer value 3). We simply don't have any way to represent 5!

Formally, an Arithmetic operation is said to *overflow* when the full integer result cannot fit within the word size limits of the data type.

If we went ahead with it, 3 + 2 is 011 + 010 = 101 in binary, which is -3 in our current system. But that's not at all the value we expect.

The way we can detect this, though, is by noting that we added two positive numbers (both of their leading bits were *0*s which is always positive values in Two's Complement systems), but the result was negative (since its leading bit was positive, which is always negative values in Two's Complement Systems).

**So, anytime we add two positive values and come up with a negative result, we have an overflow condition**!

**Likewise, the other way around, if we add two negative values and come up with a positive result, this too would be an overflow condition**!

## Multiplication

Multiplication is slower than addition, subtraction, bit-level operations and bit shifting. Specifically, an Intel Core i7 Haswell will use 3 clock cycles for multiplication where all of the above instructions takes only 1 clock cycle.

Many compilers attempt to optimize this away by replacing multiplications by constant factors with combinations of shift and addition operations.

## Division

Division far slower than multiplication, even. This typically requires something like 30 or more click cycles.

## Floating Point

This representation encodes rational numbers of the form *V = x * 2<sup>y</sup>*

With IEEE Standard 754, we've gotten a carefully crafted standard for representing floating-point numbers and the operations performed on them.

### Fractional Binary numbers

Consider this:

*0110.1000*.

The "." decimal point in the middle is called the *radix*.

The first to the left of the radix is binary for the number 6.

But then for everything to the right of the radix, you divide them by 2, 4, 8, 16, .... So in this example, it's 1/2 0/4, 0/8, 0/16. Notice that you start counting from 2, not 1!

Ultimately, this is binary for the decimal number *6.5*.

If it were:

*0110.1100*.

The fractional part to the right of the "." would be 1/2 + 1/4. 1/2 + 1/4 = 3/4. The decimal number would then be *6.75*

#### What about things like 8.1 (10ths as fractions)

Since we start from 2, 4, 8, 16, ... In the binary fractional representation, we have no easy way of displaying "1/10".

Instead, we have to *approximate* it. We do that iteratively.

1. We start with 0.1. That is equivalent to 1/2 (because we start from 2). That is too large, so we try again.
2. We try 0.01. That is equivalent to 1/4 (because we are at 2, 4). That is also too large, so we try again.
3. We try 0.001. That is equivalent to 1/8 (because we are at 2, 4, 8). That is also too large, so we try again.
4. We try 0.0001. That is equivalent to 1/16 (because we are at 2, 4, 8, 16). That is too small! So we know it is somewhere in-between step (3) and step (4).
5. We will now keep the 1/16 but add together an event smaller fraction (such as 1/32) and continue inching closer on an appromixation that is "close enough".

The more digits we add, the closer the approximation. For something like 8.1, we will need great precision. But for other things, like *.5*. we can do it really efficiently.

### How to partition the full binary expression (e.g. how to know where to place the radix)

A value such as *0110.1000* doesn't exist in binary since binary cannot include the radix but instead only 0's and 1s.

So, what you do is you specify the number of bits to use for the integer part (left of the radix) and the number of bits to use for the fractional part (right of the radix).

And then, when given the binary string, you let the interpreter/program know where the radix is (e.g. how many bits to use for the integer part).

## Rounding

Since we can only approximate real arithmetic, we generally want some systematic way of finding the closest matching value that can be represented in the desired floating-point format.

IEEE has some default rounding mdoes.

The default is called Round-to-even.

## Single-Precision vs Double-Precision

These are IEEE words.

### Single-Precision

This is a floating point standard representation that requires a 32-bit word, which may be represented as numbered from 0 to 31, left to right:

- The first bit is the *sign bit*, S.
- The next eight bits are the *exponent* bits, *E*
- The final 23 bits are the *fraction*, *F*:

This is the full form, using the variables defined above:
SEEEEEEEEFFFFFFFFFFFFFFFFFFFFFFF

So, the radix is baked-in in that it is always predetermined which bits are what.

### Double-Precision

This requires a 64-bit word, which may be represented as numbered from 0 to 63, left to right.

- Again, the first bit is the *sign bit*, *S*.
- Then, the next eleven bits are the *exponent* bits, *E*.
- The final 52 bits are the *fraction*, *F*:

This is the full form, using the variables defined above:

SEEEEEEEEEEEFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF

So as you can see, this does allow for greater precision.

## Floating Point in C

In C, we have two different floating-point data types:

`float` and `double`.

For machines that support IEEE floating point (which is basically any machine in the world), these data types correspond to single- and double-precision floating point respectively. And, these machines use round-to-even rounding mode.

There are no standards saying that the host machine of the C program must conform to the IEEE standard, so special IEEE 754 values such as `NaN` may never occur, or the rounding may be different than for IEEE-based machines.

## Summary

Beware of overflow when doing stuff like *x * x* and know that in binary, floating point will always be an approximation. Enjoy how awesome Two's-Complement tackles addition and subtraction of signed values.