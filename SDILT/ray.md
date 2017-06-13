# Rays

> Notes on rays

A ray has an **origin Point** and a **direction Vector**.

The ray equation is written as: *o + td*.

- *o* is the origin of the ray.
- *d* is the direction that the ray fires in.
- *t* is a scalar constant that uniquely determines points along the ray.

In an Orthonormal coordinate space, the kind we use for the raytracer, any fired ray will have this origin *position* and the direction in world coordinates (*not* camera coordinates):

```math
px * u + py * v - zoom * w
```

Where *px* and *py* is the center of a pixel (x,y).

## Checking whether a light source illuminates a hit point (The Shadow Ray)

To check whether a light source at point *l* illuminates a hit point *p*, we calculate the ray going from *p* towards *l*. **This is called the Shadow Ray**.

In other words, you fire a ray *from* the hitpoint *to* the light source.

**To do this, you call the hit functions of all objects in the scene like in the basic ray-tracing algorithm, but you stop checking as soon as you have found a hit with a distance smaller than that of the light source. (Because then we know that the light is blocked)**.

### Shadow Ray signature

The Shadow Ray is given by the origin *p* and the direction vector *D p l* (The vector going from *p* to *l*).

### If a light source is NOT blocked by another shape

We calculate the angle, φ, at which the light is hitting the hit point.
We use the normal vector, *n*, that is returned by the hit function.
Then we take *cos* to that angle.

Here's how we compute it:
`c = cos φ = n * (D p l)^`.

In words, we compute the **dot product** of the normal vector *n* and the normalised direction vector of the shadow ray (*(D p l)^*).

**But Beware**:
Make sure that the normal vector *n* points in the right direction!

To do that, take the dot product of the normal vector, *n* and the ray direction vector, *d* and see if it is > 0. **If it is > 0, the vector points in the wrong way**! **In that case, use -n instead of n in the calculation above**.

### If c <= 0

If the value is <= 0, the light from the light source *does* hit the hit point, **but from the wrong side**. An example could be that the light source is inside a sphere but the camera is outside of it.

**We treat this in the same way as if the light source was blocked**.

### If c > 0

If c > 0, the light source illuminates the hit point *p*.
But also, c determines **how much*' the light source will illuminate the hit point.

### Changing the color value to take light into account

When you have c, cosinus to the angle at which the light source is hitting the hit point, you need to perform the following operations to estimate the correct color value:

1. Multiply the color of the material with *c*. In this respect, *c* determines *how much* the light illuminates the hit point.
2. To take the lights color and intensity into account, pointwise multiply the color **of the light source** with the color of the material.

### If there are multiple light sources

Say we have 4 light sources:

1. First we check if any of them is blocked. For those that are, do nothing.
2. Then we compute *c* for all of them.
3. For those where c <= 0, do nothing.
4. For the remaining ones, (say we have two positive c values, *c1* and *c2* and an ambient light source *a*) the RGB color values are computed like this:
	- ```math
		r = rm * (c1 * i1 * r1 + c2 * i2 * r2 + ia * ra)
		g = gb * (c1 * i1 * g1 + c2 * i2 * g2 + ia * ga)
		b = bm * (c1 * i1 * b1 + c2 * i2 * b2 + ia* ba)
		```
		In words, we multiply the individual r, g and b values of the material with each *c* value multiplied the its intensity and its color added to one another.

## Beware: Hitpoints may be slightly underneath the shape surface

The hit point, *p* may be slightly underneath the surface of the shape! The problem: If we check for hits of the shadow ray, we would find a hit with the same shape again!

To avoid it: Move the hit point *slightly* above the surface of the shape using the normal *n* or *-n* if the dot product of the normal vector, *n* and the ray direction vector, *d* is > 0:

`p' = p + epsilon * [n|-n]` where epsilon is a very small number like *10^-6*.
So, in words, we move the point in the direction of the normal vector juuust a bit.