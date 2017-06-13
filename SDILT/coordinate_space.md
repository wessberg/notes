# Coordinate spaces

> Notes on Coordinate spaces

## Cartesian coordinate spaces

A 3D cartesian coordinate space has an x, y and z-axis. Coordinates are given using floating point numbers with the origin in [0.0, 0.0, 0.0].

### Left-handed coordinate system vs Right-handed coordinate system

In left-handed coordinate systems, the z-axis points into the screen.
In right-handed coordinate systems- the z-axis points out of the screen.

**The Ray-tracer project uses a right-handed coordinate system**.

## Orthonormal coordinate spaces

An Orthonormal coordinate space is both orthogonal and normalised.
A 3D orthonormal coordinate space consists of **3 mutually orthogonal and normalised vectors - one for the x, y and z-axes**.

These together form a right-handed coordinate system.

### Setting up an orthonormal coordinate system

This is typically done using a direction vector *d* and an *up* vector *u*.

- The direction vector is typically the *z* coordinate axis (the direction the camera is looking at).
- The *up* vector is typically a vector that represents what is considered "up" in the scene. **This is normally (0, 1, 0)**.

All in all, it is set up in the following way:

```math
c = d^ (The normalized direction vector d)
b = u × c
a = c × b
```

The reason for taking the cross product is to make sure that *a*, *b* and *c* are **orthogonal to each other**.

Both *b* and *c* are normalised to ensure that all axes are normalised.

## Axis-Aligned

= All edges pare parallel to one of the coordinate axes.