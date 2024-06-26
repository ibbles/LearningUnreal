Particle simulations in Unreal Engine.
Replaces the old Cascade system.

A visual effects platform.

The basis for [[Niagara Fluids]], a fluid simulation package in Unreal Engine.

# Pieces

- [[Niagara System]]: Container for emitters and such. Lives as an [[Asset]] in the [[Content Browser]]. Can be created or spawned in a [[Level]].
- [[Niagara Component]]: An instantiation of a [[Niagara System]] in a [[Level]].
- [[Niagara Emitter]]: Creates new particles, either when the particle system is spawned or continuously over time. Part of a [[Niagara System]], which can have many emitters.
- Particle: A particle is emitted by a [[Niagara Emitter]] and holds a collection of variables. Often associated with a [[Niagara Renderer]].
- [[Niagara Module]]: A piece of code that manipulates the state of the particle system.
	- An emitter contains a bunch of modules, some that run at emitter spawn, some that run when a new particle is spawned, and some that run on each tick to update the particles.

The main parts from a designer point of view are
- System
- Emitter
- Particle

A [[Niagara System]] contains [[Niagara Emitter]]s that contain particles.

Each of these can have [[Niagara Module]]s associated with them that perform some action on spawn/emit or on update (tick).

Data available in one level is available to all instances at the lower levels.
For example, all Emitters in a System can access that System's data, and all Particles in an Emitter can access both the Emitter's and the System's data.

# References

- [_Creating Fluid Simulation in UE5 | Inside Unreal_ by Epic Games, 2022 @ youtube.com](https://www.youtube.com/watch?v=k7WLE2kM4po)
- [_Creating Visual Effects_ @ docs.unrealengine.com](https://docs.unrealengine.com/5.0/en-US/creating-visual-effects-in-niagara-for-unreal-engine/)
- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
- [_Intro To Niagara_ by Epic Online Learning, James Hill @ dev.epicgames.com/tutorials 2023 UE5.2](https://dev.epicgames.com/community/learning/tutorials/8B1P/unreal-engine-intro-to-niagara)
