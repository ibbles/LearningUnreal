
A collision shape is an invisible volume used with a [[Static Mesh]] to generate [[Collision]].
It is often a much simpler mesh or a collection of primitives shapes,
since doing collision detection against the triangles of the [[Static Mesh]] is often too computationally expensive and not necessary.
The collision shape should be as simple as possible.
A collision shape often consists of zero or more boxes, spheres, and capsules.
Sometimes convex meshes or k-dops.
A collision mesh cannot be concave, it must be convex.
(
Is this still true?
)

In the [[Static Mesh Editor]] we can visualize the collision shapes with Main Tool Bar > Collision > Simple Collision or Complex Collision.
We can also visualize the collision shapes from the [[Level Viewport]] by enabling the Player Collision [[View Mode]].

The number of collision primitives is shown in the status text printed in the top-left corner of the [[Viewport]] in the [[Static Mesh Editor]].

If you really need every triangle in the [[Static Mesh]] to be used for collision detection then you can set [[Details Panel]] > Collision > Collision Complexity to Use Complex Collision As Simple.
This will use the complex collision data also when the simple collision shapes are requested.

# Creating Collision Shape

We can add collision shapes to a [[Static Mesh Asset]] in the [[Static Mesh Editor]].
They are added from the Top Menu Bar > Collision menu.
Here we also have the Remove Collision entry to remove all the collision shapes and start over.

When adding a new collision shape it will be created to encompass then entire mesh.
Sometimes this is good enough.
Sometimes we want to scale and position the collision shape to better fit some part of the [[Static Mesh]],
and then add additional collision shapes for the other parts of the [[Static Mesh]].


The Auto Convex Collision tool creates a convex mesh based on the triangles of the [[Static Mesh]].
This can produce a very accurate collision mesh, but is often more computationally expensive compared to shape primitives.
When Top Menu Bar > Collision > Auto Convex Collision option is selected the Convex Decomposition panel opens.
Here we can control how accurate we want the generated convex mesh to be.
- Hull Count: The number of convex meshes. Since this can be larger than one it is possible to approximate non-convex shapes with a collection of convex meshes.
- Max Hull Verts: The maximum number of vertices that is allowed per convex hull.
- Hull Precision: How closely the convex hulls should match the vertices of the [[Static Mesh]]. I don't know what this number means. What is the unit?

Click Apply to generate the meshes and see the result as green lines in the [[Viewport]], if the Main Tool Bar > Collision > Simple Collisions view has been enabled.

Start with low numbers, e.g. four hulls and 20-is vertices and increase as needed.
Experiment with Hull Precision because I have no idea what this means.
Higher values for Hull Count and Max Hull Verts will have a negative impact on runtime performance.
Higher value for Hull Precision will affect generation time when you click apply but not runtime performance.


# Importing Collision Shapes For Static Mesh Asset

When importing a [[Static Mesh]] Unreal Engine follows a set of conventions to create collision shapes from a subset of the triangles in the source asset.
These are call UBX, UCX, and USX.
- UBX: Box.
- UCX: Custom.
- USX: Sphere.

To create a box collision shape for a mesh in a 3D modeling software create a regular box shape submesh and name it with the `UBX_` prefix and a counter as suffix, counting the number of collision shapes for a particular mesh.
Between the prefix and suffix should be the name of submesh that the collision shapes belong to.
The collision submeshes should be children of the main submesh.
For example, here is the submesh hierarchy for a mesh named `SM_Chair` with two box collision shapes, where Root and Geometry are 3D modeling software specific and may vary from software to software:
- Root
	- Geometry
		- `SM_Chair`
			- `UBX_SM_Chair_01`
			- `UBX_SM_Chair_02`

When exporting, ensure that both the main submesh and the collision shape submeshes are selected.
When importing into Unreal Engine, make sure FBX Import Options > Mesh > Auto Generate Collision is disabled.

See also [[Import Static Mesh]]


# References

- [_Becoming an Environment Artist in Unreal Engine_ > _Collison Setup_ by Epic Online Learning @ dev.epicgames.com/courses 2022 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/jMJ/unreal-engine-collison-setup)
