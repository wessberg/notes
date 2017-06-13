# Reflection

> Notes on Reflections

The material information returned by the hit function does not only give a color but also information about its reflectiveness.

**This is a float between 0 and 1 that determines how much of the color of the hit point is determined by reflection**.

- 1 is a perfect mirror.
- 0 is a perfectly matte surface.

## Max Reflectivity

When we perform the basic ray-tracing algorithm, we call it recursively to take reflection into account. But in order for it not to continue forever, we set a *max_reflect* parameter that serves as an upper bound for how deep we allow the recursion to continue.