A Static Mesh Asset is a type of [[Asset]] that contains a triangle mesh model.
The static part means that the triangles can't move relative to each other.
The mesh in its entirety may move through the level, but it always moves as a single unit.
The alternative is a [[Skeletal Mesh Asset]], which can deform.

Opening a Static Mesh Asset opens the Static Mesh Editor.
It contains a Viewport showing the mesh, a Details panel where we can edit the properties of the mesh, and a Socket Manager where we can define attachment points on the mesh.

The Viewport has many of the same visualization modes and tools available to the main level [[Viewport]].
It can also display some statistics about the mesh, such as the number of vertices and triangles.

A Static Mesh can have one or more Material Slots.
(
Can it have 0?
)
A Material Slot is a mapping from a [[Material]] to a set of triangles that use that Material.
From the Material Slot Widget Row we can select which Material to use for that slot, open the [[Material Editor]] for the selected Material, and find the Material Asset in the Content Drawer.

A Static Mesh Asset can contain vertex colors.
This is a color assigned to each vertex.
While the vertex color can be rendered as-is, it is more common to use it to control some other aspect of the rendering through the [[Material]], such as which texture to sample from.

