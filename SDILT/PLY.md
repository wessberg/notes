# PLY files

> Notes on PLY files

PLY is the input format for triangles meshes in the course.

We parse it into a data structure we can work with.

## Properties

Throughout the PLY files, any elements (*vertex* or *face*) may have mor properties. For example, a normal vector may be associated (which we use for smooth shading).

They may also contain texture mapping information in properties *u* and *v*.