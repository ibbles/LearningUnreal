A Niagara Renderer is a [[Niagara Module]] that lives in the Render [[Niagara Stage]].
A renderer reads particle data and produces rendering commands,  for example as sprites or meshes.

- [[Niagara Sprite Renderer]]:  Render a 2D sprite per particle.
- [[Niagara Mesh Renderer]]: Render a [[Static Mesh]] per particle.
- [[Niagara Ribbon Renderer]]: Render a ribbon along the particles.
- [[Niagara Light Renderer]]: I assume this creates a [[Light Source]] at the particle.
- [[Niagara Geometry Cache Renderer]]: Don't know.
- [[Niagara Component Renderer]]: Don't know.

Render modules have inputs that need to be bound to particle attribute.
Some some inputs there is a clear default, for example the Position input is very often bound to the Position attribute.
Some inputs are renderer specific and must be setup on the particles somewhere,
often in Particle Spawn on Particle Update.
The built-in Initialize Particle [[Niagara Module]] has a bunch of optional inputs that set renderer-specific attributes, such as Sprite Facing.
An example where we may want to bind the Position input to something other than the Position attribute is when we have multiple renderer modules in a [[Niagara Emitter]] and want to have some offset between the two.
Then we setup one to use the Position attribute and the other to use a custom attribute that we set to be close to the Position, but with some offset.

See [[Niagara Render Material]].


# Translucency And Sort Order

When using a [[Translucency|Translucent]] [[Material]] it is important to render the objects in the correct order.
(
I assume that order is back-to-front.
)
There is no good way to do this with particle systems since some particle may be behind while others are in front.
Still, we can do the best we can by setting [[Niagara Editor]] > [[Niagara Emitter]] > rendering module > Selection panel > Sort Order > Sort Order Hint so that particles that should be in the front have a higher Sort Order Hint.


# References

- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
- [_Intro To Niagara_ by Epic Online Learning, James Hill @ dev.epicgames.com/tutorials 2023 UE5.2](https://dev.epicgames.com/community/learning/tutorials/8B1P/unreal-engine-intro-to-niagara)
