Each vertex of a [[Static Mesh]] carries a Color attribute.
While this attribute can be used to color the mesh, it is often used for other purposes.
It is typically an input to the [[Material]] used to render the mesh.
It is often used to blend between two materials, for example using [[Quixel Megascans Surfaces Material Blend]] or any other type of [[Blend Material]].
The Vertex Color is read in a [[Material]] using the Vertex Color node.

The [[Mesh Paint Editing Editor Mode Vertex Color]] is used to assign vertex colors to vertices.
This is how we do [[Vertex Paint]] in Unreal Editor.

I'm not sure where the vertex color data is stored.
It doesn't seem to be in the [[Static Mesh Asset]] because a [[Static Mesh Component]] can have different vertex colors from another [[Static Mesh Component]] using the same [[Static Mesh Asset]].
It must be stored with the [[Static Mesh Component]] instance, I guess.

The Vertex Colors assigned for a particular [[Static Mesh]] instance can be copied to the [[Static Mesh Asset]] so that when new instance is created in the future (Are existing instances also updated?) then they start with the Vertex Colors that the instance has.
To copy the Vertex Colors from the instance to the [[Asset]] click the Apply button in the [[Mesh Paint Editing Editor Mode Vertex Color]] [[Editor Mode]].

[[Nanite]] enabled meshes cannot have a vertex color.
Or at the very least, one cannot use [[Mesh Paint Editing Editor Mode Vertex Color]] on such meshes.
