Each vertex of a [[Static Mesh]] carries a Color attribute.
While this attribute can be used to color the mesh, it is often used for other purposes.
It is typically an input to the [[Material]] used to render the mesh.
It is often used to blend between two materials, for example using [[Quixel Megascans Surfaces Material Blend]] or any other type of [[Blend Material]].

The [[Mesh Paint Editing Editor Mode Vertex Color]] is used to assign vertex colors to vertices.

I'm not sure where the vertex color data is stored.
It doesn't seem to be in the [[Static Mesh Asset]] because a [[Static Mesh Component]] can have different vertex colors from another [[Static Mesh Component]] using the same [[Static Mesh Asset]].

[[Nanite]] enabled meshes cannot have a vertex color.
Or at the very least, one cannot use [[Mesh Paint Editing Editor Mode Vertex Color]] on such meshes.
