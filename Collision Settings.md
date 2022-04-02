The [[Static Mesh Component]] and the [[Collision Components]], together called Colliders, has in the Details panel, a category called Collision.

Can be combined with another Static Mesh or a [[Collision Components]].

A Collider has an Object Type.
The Object Types available by default are
- World Static
- World Dynamic
- Pawn
- Physics Body
- Vehicle
- Destructible

A Collider can handle collisions with each of these types differently.

The Collision Presets can be set to one of the following:
- Block All Dynamic
  The physics engine will prevent any Dynamic object from intersecting with this Collider.
- No Collision
  Disable collision detection for this Collider.
  Setting this on a Static Mesh Component makes it a visual mesh only.
