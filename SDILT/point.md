# Points

> Note on Points

Points in #-space are modlled as three floating point numbers representing their *x*, *y* and *z* positions:

`Point: [x, y, z]`.

## The Distance function

The distance between two points is a vector. The distance function, *D*, is of type `Point -> Point -> Vector` and calculates the distance vector between points *p* and *q*.

Here's the function:

*D (p<sub>x</sub>, p<sub>y</sub>, p<sub>z</sub>)(q<sub>x</sub>, q<sub>y</sub>, q<sub>z</sub>) <=> (q<sub>x</sub> - p<sub>x</sub>, q<sub>y</sub> - p<sub>y</sub>, q<sub>z</sub> - p<sub>z</sub>)*

## The Displacement function

The displacement function, *M*, moves a point *p* along a (typically not normalised) vector *v*.

Here's the function:

*M (p<sub>x</sub>, p<sub>y</sub>, p<sub>z</sub>)(a<sub>x</sub>, a<sub>y</sub>, a<sub>z</sub>] <=> (p<sub>x</sub> + a<sub>x</sub>, p<sub>y</sub> + a<sub>y</sub>, p<sub>z</sub> + a<sub>z</sub>)*

But *M* is typically written as *p + a*, e.g. the two points added to one another.