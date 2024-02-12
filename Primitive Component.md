
Primitive Component is the base class for most meshes being rendered or used for [[Collision]].
It is a Component intended to carry geometry information.
This includes both [[Static Mesh Component]] and [[Skeletal Mesh Component]].
The Primitive Component typically doesn't carry any mesh data by itself.
Instead it points to a mesh [[Asset]] of some kind.

Primitive Component inherits from [[Scene Component]].

# Materials

A Primitive Component has a list of [[Material]] slots.
The slots available is often determined by the mesh asset selected for the Primitive Component.
The asset often come with a [[Material]] already assigned to each Material slot, but we can override it with the Set Material node.
We can create a [[Dynamic Material Instance]] for a [[Material]] using the Create Dynamic Material Instance node.

