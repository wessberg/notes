# Affine Transformations

> Notes on Affine Transformations

Transformations are our way to rotate, shear, move (translate) or scale any shape. We use it heavily within the project for positioning since all Shapes are axis aligned.

## What it is

It is a transformation that **preserves points, parallel lines and planes**. This menas that two parallel lines will still be parallel after they are transformed.

## Matrices

Matrices **represent linear maps between vectors in finite dimensions**.

### Four dimensions

We use them to map vectors **in four dimensions** to corresponding vectors, **also in four dimensions**.

### Matrix multiplication

You can multiply two matrices of dimensions *m X n* and *n X o* to produce a matrix with dimension *m X o*.

This is done **by multiplying cells in the rows of the first matrix by the columns of the second**!

They are **the dot product of the rows and columns of the matrices**.

The **Order in which we transform our matrices are important! *AX != BA*.**.

### The identity Matrix

Identity matrices are the units of matrix multiplication.
If you multiply matrix *A* by the identity matrix *I* you get *A* back, **similarly to multiplying any number by one**.

It is a square matrix where all cells are 0 except for the top-left-to-bottom-right diagonal:

[1 0 0 0]
[0 1 0 0]
[0 0 1 0]
[0 0 0 1]

This is practical since applying transformations to shapes is done with multiplication. And by multiplying with the identity matrix, we know that stuff won't break.

### Matrix Transposition

To transpose a matrix into a new matrix, mirror the matrix around the top-left to bottom-right diagonal.

### Inverse Matrices

A square inverse matrix *A<sup>-</sup>* is a matrix designed such that *A<sup>-</sup>A = AA<sup>-</sup> = I*

It is like **Matrix division**. You divide something by it's inverse and get a unit back. It's like writing *1/a*.

## Transformation Matrices

An affine transformation applied to a point *x, y, z* generates a new point *x', y', z'* according to the linear equations:

*x' = h<sub>xx</sub>x + h<sub>yx</sub>y + h<sub>zx</sub>z + o<sub>x</sub>*

*y' = h<sub>xy</sub>x + h<sub>yy</sub>y + h<sub>zy</sub>z + o<sub>y</sub>*

*x' = h<sub>xz</sub>x + h<sub>yz</sub>y + h<sub>zz</sub>z + o<sub>z</sub>*

## What's so great about transformation matrices

What it comes down to is the fact that you can compose any amount of matrices with one another.

That way, you can combine all the transformations you want which is important for our case where objects are frequently transformed, scaled AND rotated at the same time.

## Translation

A translation multiplication matrix looks like this:

```math
T = [1 0 0 ox]
    [0 1 0 oy]
		[0 0 1 oz]
		[0 0 0 1]
```

Where *ox*, *oy* and *oz* are the offset that the point should be moved in world coordinates.

## Scaling and reflection

A scaling transformation scales a shape along the x-, y or z axis. It has the form:

```math
S = [hxx 0   0   0]
    [0   hyy 0   0]
		[0   0   hzz 0]
		[0   0   0   1]
```

Where *h<sub>xx</sub>* scales the form along the x-axis, *h<sub>yy</sub>* along the y-axis, and *h<sub>zz</sub>* along the z-axis.

## Mirroring - special case of scaling

If we use negative coordinate scalar, we will mirror any shape across its coordinate plane.

## Rotation

TL;DR;

## Shearing

Shearing **is the process of sliding one part of a shape along an axis**.

## Transformation Composition

As I wrote, no matter how many transformations we stick together, we will always have a single 4x4 transformation matrix.

These compositions are done **in a pre-processing step before rendering starts and thus have little impact on rendering performance**.

## The order of transformations when combining them

The order in which we combine them are important:

```math
T = Tn ... T1
T- = T-1
```

*T = T<sub>n</sub> ... T<sub>1</sub>*

*T<sup>-</sup> = T<sup>-</sup><sub>n</sub> ... T<sup>-</sup><sub>1</sub>*

So be careful of going the right direction.

## What we transform

**READ: We are NOT transforming shapes, we are transforming the rays that hit the shapes**!

### Advantages to transforming rays instead of shapes

1. Many shapes have non-trivial hit functions. They can be simplified if we can make assumptions such as the shape being centered at the origin of the coordinate system or that it is axis aligned. **This is hugely important**!
2. Some shapes (like triangle meshes) take up a lot of space in memory. By using affine transformations, it is easy to clone shapes and modify the clones with respect to a single parent shape.

## The translation column

The fourth column is the "translation column" - it handles translation of points by chaking the position of the points.

**But for a vector, position is irrelevant**. So, we discard the translation column when we transform vectors.