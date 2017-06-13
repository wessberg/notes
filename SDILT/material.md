# Material

> Notes on Reflections

When a hit functions finds that a ray hits the surface of a shape, it also has to return which color the shape has at the hit point (through a `Material`).

In addition to this, it also has to provide information about **how reflective the surface is** at that point.

## Reflective Factor

This is a `float` between 0 and 1 indication how much a color should reflect on its surroundings.

We typically call this *refl*.

## Material type

To describe the combination of a color and its reflectivity.

`type Material = (col: Color) (refl: float)`

### Ways that materials are assigned to a shape

1. A shape is given a single material.
	- All points in the shapes surface have that material.
2. A shape is given a *texture* which assigns potentially different materials to different points on the surface of the shape.

### Representing colors as RGB values

Event though a Color is 3 `float` values, the final rendered image has to represent colors as RGB-triples (triples of integers between 0 and 255).

So we have to do this conversion.

#### How to convert from Color floats to RGB values

You can't just multiply to 255 since **you have to be aware of gamma correction**.

1. Take the square root of the individual floating point values.
2. Multiply that by 255.
3. Round to the nearest integer.

#### How to convert from RGB values to Color floats

1. Divide the individual integer values by 255
2. Take the resulting values to the power of 2.