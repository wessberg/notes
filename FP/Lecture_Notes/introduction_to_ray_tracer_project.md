# (Lecture notes) Introduction to Ray Tracer project.
> February 7th 2017

### Difficulty
Some parts are harder than others.

### Exam
- Group session of 5 minutes.
- Individual examination in 20 minutes.

You need to know your own code intimately but also have robust knowledge about all other parts.

## Technical part

### Scene
You place objects (light sources and shapes) and a camera in the scene to populate it.

### Firing a Ray
To ray-trace, you *fire* a ray through the center of every pixel.
You then check if it hits anything - and if it does -, you bounce it off to the light sources in the room.

### Anti-aliasing in ray-tracing
You do it by sending *more* than 1 ray through every pixel and taking the average.

## Mathematics
You need
- Points in 3D space. (x, y, z)
	-	Points in space with coordinates x, y and z.

- 3D vectors (direction and length)
	-	A direction from the origin to a point with coordinates x, y and z.

- Operations on points and vectors (cross product, dot product, distance, normalization, etc)

- Color representation and scaling.

- Hit detection for objects (ranges from very easy to very complex).

**A ray consists of an origin point *p* and a direction vector *p*.**

## Implicit Surfaces
Implicit surfaces are on of the harder/hardest parts of the project.

Implicit surfaces are equations in 3D space.

### Sphere equation
The equation for a sphere, for example, is:
*x<sup>2</sup> + y<sup>2</sup> + z<sup>2</sup> - r<sup>2</sup> = 0*

### Ray equation
A *ray* has the equation *p + td* where *p* is a point, *t* is a scalar distance (typically a double) and *d* is a normalized direction vector.
So, *t* is the unknown here. You already know the point and vector.

### Computing *t* with hit functions
Takes an object and a ray where the point *p* and the direction vector *d* are known.

*if* the hit function is successful, you found a *t*.
You may find multiple *t* values, for example one going in and out of a sphere.

To compute it, replace all occurrences of *x*, *y* and *z* with the corresponding points from the point *p* and the vector *d*.

Then simplify the exponents and solve the polynomial.
The smallest non-zero root (if any) is the distance the ray must travel before it intersects the sphere.

### Orders in polynomials
**Implicit surfaces can be used to describe many shapes, but not *all* polynomials are of third or even fourth order**.

The order of the polynomial is the maximum number of times a ray may hit a shape.

Hence, a plane is first order.
A torus is fourth order.
A sphere is second order (it may enter on one side, exit on the other).

**Generating hit functions are expensive. Do it as a preprocessing before rendering the scene**.

## Triangle meshes
Implicit surfaces are great for modeling shapes *exactly*.
*But*, sometimes we don't need that amount of precision.

Triangle meshes excel at modeling complex shapes (Animals, trees, etc).
But, in this case, spheres are only approximations of spheres. Because, its just triangles.

Triangle meshes are stored in `PLY`-files. They are stored as vertices and edges (and thus not as individual triangles).

## Optimization (Parallelization and acceleration structures)
To optimize rendering, you can use parallelization by splitting out the workload to parallel threads on separate CPU cores and let them work on their own individual parts. You can do this as long as the scene fits in memory.

Also, you might not need to check every ray for intersection with every object in a scene.
Instead, you can use:
- Bounding boxes
- Acceleration structures (with kd-trees)
- Only load objects into memory once.

## Object Composition (hemispheres, objects with bevelled edges, etc)
Stuff that is composed of primitive shapes (using intersection, union and difference).
For example, to bevel edges on a cube, you use spheres to do the rounding/beveling.

## Texturing
The texturing information is found in the PLY-file associated with the project.
Reflection is part of this part too, and since we only do perfect reflection, it should be easy enough.

## Object modification (skewing, scaling, etc)
Modification is handled using 4x4 translation matrixes.
These matrices can be multiplied together to create one complex transformation.

Trick: The shapes are not modified at all, but the rays are sent through lenses that make shapes look modified. Simple and efficient.

## Take-home message
- The project is really big. Don't do it alone.
- Start planning in your groups asap.
- Work as a team.
- Have your own non-trivial corner to demonstrate.
