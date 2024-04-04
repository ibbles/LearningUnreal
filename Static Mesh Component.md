A Static Mesh Component is a [[Primitive Component]] and a [[Scene Component]] that renders a [[Static Mesh Asset]] at its location.
An alternative to Static Mesh Component is Instanced Static Mesh Component.

# Events

A Static Mesh Component provides a number of [[Event]]s that the owning [[Actor]] can listen on.

## On Begin Cursor Over / On End Cursor Over

Triggered when the mouse cursor enters or exits the mesh in the [[Viewport]].
Mouse Over Events must be enabled on the [[Player Controller]].
This can be enabled from a [[Blueprint Visual Script]] with the Set Enable Mouse Over Events node.
