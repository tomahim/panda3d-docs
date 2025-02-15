.. _geom:

Geom
====

The Geom object collects together a :ref:`geomvertexdata` and one or more
:ref:`geomprimitive` objects, to make a single renderable piece of geometry.
In fact, an individual Geom is the smallest piece into which Panda will
subdivide the scene for rendering; in any given frame, either an entire Geom
is rendered, or none of it is.

Fundamentally, a Geom is very simple; it contains a pointer to a single
GeomVertexData, and a list of one or more GeomPrimitives, of various types, as
needed. All the associated GeomPrimitives index into the same GeomVertexData.

+-----------------------------------------------------------------------------+
| Geom                                                                        |
| ==============                                                              |
| GeomVertexData                                                              |
| ==============                                                              |
|                                                                             |
| =============                                                               |
| GeomTriangles                                                               |
| GeomTriangles                                                               |
| GeomTristrips                                                               |
| =============                                                               |
+-----------------------------------------------------------------------------+


The GeomVertexData pointer may be unique to each Geom, or one GeomVertexData
may be shared among many different Geoms (each of which might use a different
subset of its vertices). Also, although the GeomPrimitive objects are usually
unique to each Geom, they may also be shared between different Geoms.

Although a Geom can have any number of GeomPrimitives associated with it, all
of the GeomPrimitives must be of the same fundamental primitive type:
triangles, lines, or points. A particular Geom might have GeomTriangles,
GeomTristrips, and GeomTrifans; or it might have GeomLines and GeomLinestrips;
or it might have GeomPoints. But no one Geom can have primitives from two
different fundamental types. You can call geom.getPrimitiveType() to determine
the fundamental primitive type stored within a particular Geom.
