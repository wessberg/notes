# Lights

> Notes on Lights

A scene consists of light sources that illuminate the scene of shapes.
We have to take these lights into account when we determine the color of a pixel.

We must manipulate the color depending on which light sources illuminate the hit point **and at what angle light comes in**.

## The `Light` type

A light source is given as given by:

- A `Point` describing its location.
- A `Color`
- An *intensity*, which is a `float` between 0 and 1 describing how bright the light source is.

`type Light = (pos: Point) (col: Color) (intensity: float)`

## The `AmbientLight` type

An `AmbientLight` is simply a light without a location.

`type AmbientLight = (col: Color) (intensity: float)`.

We expect any shape to always be illuminated by the ambient light source.