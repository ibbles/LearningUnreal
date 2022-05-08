Every object that has a position in the world has a transformation.
The transformation describe an object's location, rotation, and scale.
Transformations can be chained, one being applied on the result of the one before it.
We call this a **transformation hierarchy**, since each step can branch into many child transforms.
The Scene Components of an Actor is an example of a transformation hierarchy.
We call each level in the hierarchy a coordinate frame, frame, or a space.
Each transform can be seen as a local transformation or a global transformation.
The **local transformation** tells you where the transformed object is relative to the transformation's parent transformation.
Also called the local space of the parent.
The **global transformation** tells you where the transformed object is relative to the world.
Also called world space or the world coordinate system.
The **world** is the coordinate system you end up in if you walk up the parent chain until there are no more parents.

Each object has a point that defines its **origin**.
An object that has the **identity transformation**, i.e. a no-op transformation, is placed with its origin at the origin of the parent frame.
An Actor's origin is marked by a white sphere icon.
If you ask for an [[Actor]]'s location then you will get the position of the Actor's [[Root Component]] origin in world space.

# Helpful Functions
There may be more useful stuff in [[Blueprint Utility And Math Functions And Techniques]].

- Find Look At Rotation(`Vector Start`, `Vector Target`) → `Rotator`
  - Return the [[Rotator]] that rotates `Start` to `Target`.
- [[Actor]] > Get World Location
  - Return the location in world space of the [[Root Component]] of the [[Actor]].
- Restrict a [[Rotator]] to only only axis by splitting it and only passing one of Roll, Pitch, Yaw to a Make Rotator node.
- We can call Get Forward Vector, Get Right Vector, and Get Up Vector on many things.

