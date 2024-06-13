A Static Mesh Component is a [[Primitive Component]] and a [[Scene Component]] that renders a [[Static Mesh Asset]] at its location.
An alternative to Static Mesh Component is [[Instanced Static Mesh Component]].


# Selection

With a [[Static Mesh Component]] (I think, or is this only for [[Static Mesh Actor]]?) selected in the [[Level Viewport]] we can right-click > Select > Static Meshes > {Select Matching (Selected Classes), Select Matching (All Classes)} to select all [[Static Mesh|Static Meshes]] using the same [[Static Mesh Asset]].

 Select all [[Actor]]s using a particular [[Asset]] with [[Content Browser]] > Asset Actions > Select Actors Using This Asset.


# Operations

With one or more [[Static Mesh Component]] (I think, or is this only for [[Static Mesh Actor]]?) selected in the [[Level Viewport]] we can select a [[Static Mesh Asset]] in the [[Content Browser]], right-click the [[Static Mesh Component]] and select Replace Selected Actors With to make the [[Static Mesh Component]] reference the [[Static Mesh Asset]] selected in the [[Content Browser]] instead of whatever [[Static Mesh Asset]] it was referencing before.
This is useful when replacing block-out meshes with art assets.

# Events

A Static Mesh Component provides a number of [[Event]]s that the owning [[Actor]] can listen on.

## On Begin Cursor Over / On End Cursor Over

Triggered when the mouse cursor enters or exits the mesh in the [[Viewport]].
Mouse Over Events must be enabled on the [[Player Controller]].
This can be enabled from a [[Blueprint Visual Script]] with the Set Enable Mouse Over Events node.


# References

- [_Advanced Skill Sets for Environment Art_ > _Pivots, Placement, and Predefined Spaces_ by Epic Online Learning 2022 UE4.27](https://dev.epicgames.com/community/learning/courses/Qwa/unreal-engine-advanced-skill-sets-for-environment-art/MlZe/pivots-placement-and-predefined-spaces)

