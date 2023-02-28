This note talks about animating a [[Skeletal Mesh]] by direct positioning of the constituent bones.

[[Animation]] is typically baked into an [[Animation Montage]].
Another option is to use per-frame logic to set the transforms of individual bones of a [[Skeletal Mesh]].
This is possible if we use [[Poseable Mesh Component]] instead of [[Skeletal Mesh Component]] to add the [[Skeletal Mesh Asset]] to the [[Actor]].

A [[Skeletal Mesh]] contains a hierarchy of named bones in a transformation parent / child tree.
We can call `Set Bone Location|Rotation|Scale|Transform By Name` on a [[Poseable Mesh Component]] to place that particular bone.

Note that moving one bone will move all the child bones.
So if you place a branch or child bone where it should be and then place a parent node then the that will misplace  the already placed bone.
So always order the bone updates in root-to-leaf order.

We can see the bone hierarchy in the [[Skeletal Mesh Editor]], in the Skeleton Tree panel.
Selecting a bone will show the bone location and child-bones in the [[Viewport]].
Bones that don't have any associated triangles have a darker color in the Skeleton Tree panel.
(
I thought. Not so sure anymore.
)

We can visualize which vertices belong to a bone by enabling [[Viewport]] > Character > Mesh > Select Bone Wights.
The vertices that are affected by the bone are highlighted in red.
The vertices that are not affected by the bone are highlighted in magenta.

We can find the world location of a bone by clicking the box / sphere icon next to Location, Rotation, or Scale in the Details panel until it shows the sphere.
We can then Shift+RMB on e.g. Location to copy it and then Shift+LMB to paste into e.g. any kind of Scene component to place it at the same place.

View frustum culling is an issue.
If we move bones outside of the bounding box of the [[Skeletal Mesh]] and then place the camera so that the vertices of the bone is visible but the original bounding box of the [[Skeletal Mesh]] is not then the vertices will not be rendered because the engine will believe that the vertices are all outside of the view frustum.