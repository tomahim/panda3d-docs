.. _multiple-texture-coordinate-sets:

Multiple Texture Coordinate Sets
================================

In addition to simple texture transforms, it is also possible to have more
than one set of texture coordinates on a model. Panda allows you to define as
many different sets of texture coordinates as you like, and each set can be
completely unrelated to all of the others.

When you have multiple texture coordinate sets (sometimes called multiple UV
sets) on a model, each set will have its own name, which is any arbitrary
string. The default texture coordinate set has no name (its name is the empty
string).

Normally, you create multiple texture coordinate sets in the same modeling
package that you use to create the model. Not all modeling packages, and not
all Panda converters, support multiple texture coordinates. In fact, as of the
time of this writing, only the Panda3D 1.1 version (or newer) of the maya2egg
converter is known to convert multiple texture coordinates into Panda.

If you happen to have a model with multiple texture coordinate sets, you can
specify which set a particular texture should use by calling
``TextureStage.setTexcoordName("name")``.

WARNING

``TextureStage.setTexcoordName("texcoord.name")`` Will cause the default
unnamed texcoord to be used.

``TextureStage.setTexcoordName("name")`` Is correct.

Remember, a TextureStage is used to apply a texture to a model, and so every
texture will have an associated TextureStage (though most textures just use
the default TextureStage). If you do not call this method for a particular
TextureStage, the default behavior is to use the default, unnamed texture
coordinate set.

The different TextureStages on a model might share the same texture coordinate
sets, or they might each use a different texture coordinate set, or any
combination.
