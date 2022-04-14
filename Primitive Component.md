Primitive Component is the base class for most meshes being rendered.
This includes both [[Static Mesh Component]] and [[Skeletal Mesh Component]].
The Primitive Component typically doesn't carry any mesh data by itself.
Instead if points to a mesh [[Asset]] of some kind.

# Materials
A Primitive Component has a list of [[Material]] slots.
The slots available is often determined by the mesh asset selected for the Primitive Component.
The asset often come with a [[Material]] already assigned to each Material slot, but we can override it with the Set Material node.
We can create a [[Dynamic Material Instance]] for a [[Material]] using the Create Dynamic Material Instance node.

