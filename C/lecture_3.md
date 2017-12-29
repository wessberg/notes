# Lecture 2

> CS:APP, Chapter 2.2

## Integer Representations

There are two primary ways bits can be used to encode integers - one that can only represent nonnegative numbers, and one that can represent negative, zero, and positive numbers.

### Integral Data Types

Integral data types are ones that represent *finite* ranges of integers. In the notes for the previous lecture, their sizes in terms of bytes were written.

Depending on the amount of the allocated bytes, the differnet sizes allow different ranges of values to be represented.

**These ranges are not symmetric**! The range of negative numbers extends one further than the range of positive numbers.

Here is a table of the ranges for 32-bit:

|         Data type         |            Minimum             |          Maximum          |
|---------------------------|--------------------------------|---------------------------|
|      `[signed] char`      |             -128               |            127            |
|      `unsigned char`      |               0                |            255            |
|          `short`          |            -32,768             |           32,767          |
|      `unsigned short`     |               0                |           65,535          |
|           `int`           |         -2,147,483,648         |        2,147,483,647      |
|         `unsigned`        |               0                |        4,294,967,295      |
|          `long`           |         -2,147,483,648         |        2,147,483,647      |
|      `unsigned long`      |               0                |        4,294,967,295      |
|         `int32_t`         |         -2,147,483,648         |        2,147,483,647      |
|        `uint32_t`         |               0                |        4,294,967,295      |
|         `int64_t`         |   -9,223,372,036,854,775,808   | 9,223,372,036,854,775,807 |
|        `uint64_t`         |               0                |18,446,744,073,709,551,615 |

Here is a table of the ranges for 64-bit:

|         Data type         |            Minimum             |          Maximum          |
|---------------------------|--------------------------------|---------------------------|
|      `[signed] char`      |             -128               |            127            |
|      `unsigned char`      |               0                |            255            |
|          `short`          |            -32,768             |           32,767          |
|      `unsigned short`     |               0                |           65,535          |
|           `int`           |         -2,147,483,648         |        2,147,483,647      |
|         `unsigned`        |               0                |        4,294,967,295      |
|          `long`           |   -9,223,372,036,854,775,808   | 9,223,372,036,854,775,807 |
|      `unsigned long`      |               0                |18,446,744,073,709,551,615 |
|         `int32_t`         |         -2,147,483,648         |        2,147,483,647      |
|        `uint32_t`         |               0                |        4,294,967,295      |
|         `int64_t`         |   -9,223,372,036,854,775,808   | 9,223,372,036,854,775,807 |
|        `uint64_t`         |               0                |18,446,744,073,709,551,615 |

## Unsigned Encodings

### Bit vectors

We write a bit vector as either *x* with an arrow on top of it or as: *[x<sub>w - 1</sub>, x<sub>w - 2</sub>, ..., x<sub>0</sub>]*.

**If we treat the bit vector *x* as a number in binary notation, we obtain the *unsigned* interpretation of *x***.

### Unsigned interpretation

Here, each bit *x<sub>i</sub> has a value 0 or 1*

### Binary-to-unsigned (B2U<sub>w</sub>)

We can express a bit vector interpreted as a number written in binary as a function *B2U<sub>w</sub>* where *w* is the length.

What it does it that it maps strings of zeros and ones of length *w* to nonnegative integers:

*B2U<sub>4</sub>([0001]) = 0 * 2<sup>3</sup> + 0 * 2<sup>2</sup> + 0 * 2<sup>1</sup> + 1 * 2<sup>0</sup> = 0 + 0 + 0 + 1 = 1*

*B2U<sub>3</sub>([0101]) = 0 * 2<sup>3</sup> + 1 * 2<sup>2</sup> + 0 * 2<sup>1</sup> + 1 * 2<sup>0</sup> = 0 + 4 + 0 + 1 = 5*

*B2U<sub>3</sub>([1011]) = 1 * 2<sup>3</sup> + 0 * 2<sup>2</sup> + 1 * 2<sup>1</sup> + 1 * 2<sup>0</sup> = 8 + 0 + 2 + 1 = 11*

*B2U<sub>3</sub>([1111]) = 1 * 2<sup>3</sup> + 1 * 2<sup>2</sup> + 1 * 2<sup>1</sup> + 1 * 2<sup>0</sup> = 8 + 4 + 2 + 1 = 15*

And this is basically it! This way, we can easily convert from binary to decimal, - for nonnegative integers (which is fine for the unsigned data types).

## Twos-Complement Encodings

For many applications, we wish to represent negative values as well.

**The most common computer representation of signed numbers is known as *two's-complement***.

**This is defined by interpreting the most significant bit of the word to have negative weight!**

We can express this with the function *B2T<sub>w</sub>* ("binary-to-two's-complement).

### Binary-to-Two's-Complement

For the bit vector *x = [x<sub>w - 1</sub>, x<sub>w - 2</sub>, ..., x<sub>0</sub>]*, the most significant bit is *x<sub>w - 1</sub>*.

**This is called the sign bit**. It's weight is *-2<sup>w - 1</sup>*.

#### Sign bit

- When the sign bit is set to 1, the represented value is negative.
- When the sign bit is set to 0, the value is nonnegative.

*B2T<sub>4</sub>([0001]) = -0 * 2<sup>3</sup> + 0 * 2<sup>2</sup> + 0 * 2<sup>1</sup> + 1 * 2<sup>0</sup> = 0 + 0 + 0 + 1 = 1*

*B2T<sub>3</sub>([0101]) = -0 * 2<sup>3</sup> + 1 * 2<sup>2</sup> + 0 * 2<sup>1</sup> + 1 * 2<sup>0</sup> = 0 + 4 + 0 + 1 = 5*

*B2T<sub>3</sub>([1011]) = -1 * 2<sup>3</sup> + 0 * 2<sup>2</sup> + 1 * 2<sup>1</sup> + 1 * 2<sup>0</sup> = -8 + 0 + 2 + 1 = -5*

*B2T<sub>3</sub>([1111]) = -1 * 2<sup>3</sup> + 1 * 2<sup>2</sup> + 1 * 2<sup>1</sup> + 1 * 2<sup>0</sup> = -8 + 4 + 2 + 1 = -1*

As you can see, the *-0* parts effectively becomes 0, and hence the result is nonnegative.

So, if the sign bit is 1, it effectively says "yup, I have a (minus) sign".

Since *B2*T is a bijection - which means there is an inverse (because it is one-to-one and onto), we can also reverse it and construct a *T2B<sub>w</sub> (Two's-complement-To-Binary)* function.

### Difference between B2U and B2T

Well, basically just whether or not you place a *"-"* in front of the leading *0* or *1*. What's practical about it is that "-0" becomes 0 and hence we use can both represent negative and positive numbers using Two's-Complement.

### Usage of Two's-Complement

Event though C doesn't require signed integers to be represented in two's-complement form, most machines do (well, almost all of them).

## Converting between Signed and Unsigned

The values might change, but the bit patterns do not. So, for example, if we cast a (signed) short with a value of *12345* to an unsigned short, the value becomes *53191*. So be aware of this.

## Expanding the Bit Representation of a Number

It should always be possible to convert from a smaller to a larger data type.

### Converting an unsigned number to a larger data type

To convert an unsigned number to a larger data type, **we can simply add leading zeros to the representation**. This is known as *zero extension*.

### Converting a two's-complement number to a larger data type

To do this, we perform something called *sign extension*. Here, we **add copies of the most significant bit to the representation**.

(Note to self: So THAT is why right-shifts are arithmetic for signed values and logical for unsigned!! When unsigned, we are satisfied with leading zeros since the value is positive at all times. If signed and we added leading zeros, we would mistakenly assume that a negative number was in fact positive)

## Truncating Numbers

Sometimes we want to *reduce* the number of bits representing a number, rather than extending it.

## General advice on signed vs unsigned

If it can be avoided, don't use unsigned as it can lead to errors or vulnerabilites, especially when converting.

Most languages other than C doesn't even support unsigned!