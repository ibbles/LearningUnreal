A Niagara Namespace describes where a piece of [[Niagara]] data is stored.
It defines use case and persistent.
Data at a particular level (System, Emitter, Particle) can only be written in modules in stages at that level but can be read at all lower levels.
The Map Get and Map Set nodes read and write data, respectively.
You can change which namespace a particular read or write happens from/to by right-clicking the output or input pin and select Change Namespace.

- SYSTEM
	- Persistent data stored per [[Niagara System]].
- EMITTER
	- Persistent data stored per [[Niagara Emitter]].
- PARTICLES
	- Persistent data stored per particle.
- INPUT
	- Parameter to a [[Niagara Module]].
- LOCAL
	- Temporary variable within a [[Niagara Module]].
- OUTPUT
	- ???
- STACKCONTEXT
	- ???
- TRANSIENT
	- Temporary variables that exists for a single stage.

# SYSTEM

Data that is stored on a [[Niagara System]].
Can be written in System stages.
Can be read in System, Emitter, and Particle stages.
(
Are there other stages it can be read in?
)

# EMITTER

Data that is stored on a [[Niagara Emitter]].
Can be written in Emitter stages.
Can be read in Emitter and Particle stages.

# PARTICLES

Data that is stored per particle in a particular [[Niagara Emitter]].
Written and read in Particle stages.

# LOCAL

Variable written an read within a single [[Niagara Module]].
Not persistent, disappears as soon as the the module finishes executing.

# INPUT

When set on an output pin on a Map Get node on 

# TRANSIENT

Temporary variables that exists for a single stage.
Can be read by other [[Niagara]] modules in the same stage.
A stage is e.g. Emitter Update, Particle Spawn, or Particle Update.


# Namespace Modifier

- Module
- Initial
- Previous
- Custom


# References

- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
