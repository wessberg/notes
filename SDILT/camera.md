# Camera

## Definitions

- TS;WM: Too Short; Want More

> Notes on cameras

In order to set up a camera, we need:

- A position
- A direction that it is looking in
- The distance to the view plane
- The resolution of the image
- The size of the pixels

Here's the *Camera* type:

`type Camera = (position: Point) (lookat: Point) (up: Vector) (zoom: float) (width: float) (height: float) (resX: int) (resY: int)`.

Here's what the different things are:

- A camera is given by its position (`position: Point`)
- The center of the view plane (`lookat: Point`)
- A Vector that points upwards (`up: Vector`)
- A zoom value that represents the distance to the view plane (`zoom: float`)
- The width of the view plane (`width: float`)
- The height of the view plane (`height: float`)
- The resolution of the view plane (width and height in pixels) (`resX: int, resY: int`)

## Pixel width

The `pixelWidth` and `pixelHeight` of each pixel can be computed with:

```math
pixelWidth = width/resX
pixelHeight = height/resY
```

## The camera's role

The camera will fire a ray through the center of *every* pixel and **paint the pixel the colour of any shape it hits - taking shading and reflection into consideration**.

## Basic ray-tracing algorithm TL;DR

It works by shooting rays from the position of the camera through the pixels of the view plane - **one ray through each pixel**. The color of a pixel is then determined by checking which object the corresponding ray hits (if any).

## Camera setup

To set up the camera, **an orthonormal basis** is constructed by the *direction vector* between the camera position and the center of the view plane, and the up vector:

```math
w = (D lookat position)^
u = up × w
v = w × u
```

To target the center of a pixel (x,y), this algorithm can be used:

```math
px = pixelWidth * ( x - (resX / 2) + 0.5 )
py = pixelWidth * ( y - (resY / 2) + 0.5 )
```

## Basic ray-tracing algorithm TS;WM

To render an image of a scene, **we have to calculate the color of each pixcel of the image**.

1. First, we clculate the ray that corresponds to the pixel (see above).
2. Then **we use that ray to check whether it hits any shapes in the scene**.
3. To do this, each shape must provide a *hit function*.
	- A hit function takes a ray as input, checks whether it hit the shape, **and if it does, it returns detailed information about the intersection (or *hit*) that is closest to the origin of the ray**. Such information is:
	  - *t*: The distance of the hit from the origin of the ray.
		- *v*: The normal vector of the hit point (we need it for shading and reflections)
		- *m*: The *material* of the shape at the hit point. This includes the reflectivity.
4. The hitpoint, *p* can be be calculated **by using the distance *t* returned by the hit function and plugging it into the ray equation *p = o + t * d***

So, to determine the color of a given pixel, call the hit function of *every* object in the scene (not taking acceleration structures into account). The hit that will determine the color of the pixel is the one that is closest to the camera (the one with the smallest distance *t*).

That color might be the color of the closest hit or black if there is none, but it should also take lights, reflections, etc, into account.

**We need to perform the ray tracing algorithm recursively to take reflections into account**:

- Calculate the RGB color value. Shoot a secondary ray with p' - the point that is slightly moved above the surface - as origin and with `d' = d - (2 * (n * d)) * n` as direction vector (normalise it first).
- The recursively call the ray-tracing algorithm on this secondary ray which will give us another RGB color value (r', g', b').
- Then combine the two colors based on the reflectiveness *f* of the material.
- The FINAL color is then:
	```math
	rf = r * (1 - f) + r' * f
	gf = g * (1 - f) + g' * f
	bf = b * (1 - f) + b' * f
	```

We can in theory perform a slight optimization and don't do any recursion of the reflectivity of the material is 0.

## Taking transformations into account

Instead of checking if a ray hits a shape, we modify the ray using transformation matrices and check if this transformed ray hits the shape.

**IF** the transformed ray hits the shape, then we know that the original ray will hit the *transformed* shape.

Read this again. It makes sense after reading it and thinking about it.

In practise:

Traditionally, a ray with the origin *o* and direction *d* hits a point *p* if `o (t*d) = p` for some distance *t*.

But now, we check if we hit a shape **that has been transformed by the transformation matrix *T*.**: `o (t * d) = Tp` where *p* is the hit point on the original shape.

To check if we hit the original shape **we need to transform the ray with the inverse transformation matrix *T<sup>-</sup>* using the equation: TL:DR**

## Generation of hit functions for transformed shapes in steps

1. Transform the ray using the inverse transformation matrix *T<sup>-</sup>* and check if that ray hits the original object using the **old** hit function.
2. Calculate and normalise a new hit point normal by using the inverse transposed transformation matrix on the normal obtained from the original hit function.
3. Return the distance and material from the *old* hit function along with the new normal. Your ray tracer wil ltake over shading and reflections from here completely oblivious to the fact that the shape that was hit was actually a transformed shape.

So, from reading this - we only need the new normal vector, everything else comes from the original hit function.

## REMEMBER List

- *t* is the smallest distance to the camera from something.