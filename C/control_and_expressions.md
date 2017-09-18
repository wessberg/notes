# Control and Expressions

> CS:APP (2.1, 2.2 p 92-120)

Two issues:

1. **Finite representation**
	- There is a limit to the number of integers (a max value) that can be represented on a fixed number of bytes.

2. **Representing positive and negative integers**
	- Sign and values must be represented

## Machine operations for integer arithmetic

These are supported *by* the processor:

```cli
INC, DEC, NEG, NOT, ADD, SUB, IMUL,
// Boolean operations
XOR, OR, AND,
// Bitwise shifts
SAL, SHL, SAR, SHR
```

The processor can perform **4** additions at a time.
The processor can only perform 1 multiplication and 1 division at the time.
**Division is expensive**. Latency ~3-30 units in comparison to ~1 for addition.

## Integer types

### Two's complement

The processors way to represent integer values in binary.

### Why

The beauty of two's complement is that arithmetic operations are unified.

#### Binary Representation of N bytes (with Two's complement)

#### For positive numbers

- Sign bit is 0
- Binary representation of value on `(8 * N)-1` bits

#### For negative numbers

- Binary representation of corresponding positive value on `8*N` bits.
- Invert all digits (0 becomes 1, 1 becomes 0)
- Add one

#### Example

Representing 15213:
00111011 01101101

Representing -15213:

1. 00111011 01101101 // The starting point
2. 11000100 10010010 // Negation of everything
3. 11000100 10010011 // +1

### Sign bit

The first bit of the integer representation. It is 0 if non-negative, 1 for negative.

### Signed vs Unsigned

If there is a mix of signed and unsigned values in an expression, then **signed values are implicitly cast to unsigned**!

### Beware

You may end up passing an integer value that is larger than the max value. This will segfault!
You can do this if you pass a signed language where an unsigned is expected sometimes.
So, be careful of unsigned integers!

#### Signed

#### Unsigned

### Number of bytes

- 1: char
- 2: short
- 4: int
- 4: (32 bits) or 8 (64 bits): long
- 8: long long

## Bit manipulation

