A Skeleton Asset contains a hierarchy of bones.
Vertices of a [[Skeletal Mesh]] are associated with these bones, and follow their bone when it moves.
[[Animation]] control the bones, which in turn move the vertices, which in turn moves the triangles that we can see on screen.
The bones can be visualized in the Skeleton Editor with Viewport > Character > Bones > All Hierarchy.

[[Animation Asset|Animation Assets]] often need to reference a Skeleton so that it known what the animation should animate.
Multiple meshes can use the same Skeleton Asset and [[Animation Asset]],
making is possible to re-use animation work in many parts of the project.

An [[Animation Asset]] is assigned to a Skeleton Asset in the [[Animation Graph]].

