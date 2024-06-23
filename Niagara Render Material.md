A Niagara Render Material is a regular [[Material]] that has [[Material Editor]] > [[Details Panel]] > Usage > Used With Niagara (Sprites|Ribbons|Mesh Particles) and optionally makes use of particle input nodes.
Particle input nodes read data from [[Niagara]] particle attributes.
You can find the particle input nodes under Particles when right-clicking in the [[Material]] [[Node Graph Editor]].


- Particle Color
- Particle Position WS: World space.
- Particle Radius
- Particle Relative Time
- Particle Direction
- Particle Speed
- Particle Macro UV
- Dynamic Parameter: See _Dynamic Parameter_ below.


# Dynamic Parameter

Set in the particle system.
(
How?
)

# Choosing Material

The [[Material]] to use for a particular [[Niagara Renderer]] [[Niagara Module]] can either be set in [[Niagara Editor]] > Selection panel directly or using a binding.
To use a binding create a user [[Niagara Parameter]] of type [[Material Interface]] and set render module > Selection panel > Material User Param Binding to that parameter.

# References

- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
