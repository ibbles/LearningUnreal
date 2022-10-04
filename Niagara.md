Particle simulations in Unreal Engine.
Replaces the old Cascade system.

A visual effects platform.

The basis for [[Niagara Fluids]], a fluid simulation package in Unreal Engine.

# Pieces

- [[Niagara System]]: Container for emitters and such. Lives in the Content Browser. Can be created or spawned in a [[Level]].
- [[Niagara Emitter]]: Creates new particles, either when the particle system is spawned or continuously over time.
- [[Niagara Module]]: A piece of code that manipulates the state of the particle system.
	- An emitter contains a bunch of modules, some that run at emitter spawn, some that run when a new particle is spawned, and some that run on each tick to update the particles.


# References

- [_Creating Fluid Simulation in UE5 | Inside Unreal_ by Epic Games, 2022 @ youtube.com](https://www.youtube.com/watch?v=k7WLE2kM4po)
- [_Creating Visual Effects_ @ docs.unrealengine.com](https://docs.unrealengine.com/5.0/en-US/creating-visual-effects-in-niagara-for-unreal-engine/)
