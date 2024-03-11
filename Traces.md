A Trace is a spatial query into the scene and returns the first Actor that the Trace hits.
There are different types of Traces depending on the shape:
- [[Line Trace]]
- [[Sphere Trace]]

A Trace is bounded in spacing, having both a Start and an End coordinate.

# Trace Channel


# Hit Result
- Blocking Hit: True if the Trace hit an Actor.


# Troubleshooting
If a Trace doesn't return an Actor that it should, then check the following:
- Enable Draw Debug on the Trace. Is it where you expect it to be?
- Trace Channel on the Trace node. Does it match the Actor?
- Check that the Component that Trace is supposed to hit has the Trace Responses setup correctly. (Whatever that means.)
- Check that the Static Mesh Asset, if any, has any [[Collision Shape|Collision Shapes]], or a [[Complex Collision]] set and enabled.
  - If there is no [[Collision Shape]] then one can be added with Static Mesh Editor > Top Menu Bar > Collisions > Add .+ Collision.