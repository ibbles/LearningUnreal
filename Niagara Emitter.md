A Niagara Emitter is a piece of a [[Niagara System]].
The emitter is responsible for spawning new particles and updating them each frame.
An emitter consists of [[Niagara Module|Niagara Modules]].
The modules define the behavior or the emitter.
The modules are part of different stages that run at different times:
- Emitter Spawn
- Emitter Update
- Particle Spawn
- Particle Update
- Render

The [[Niagara Module]]s within a stage are executed top to bottom.
The different stages have different responsibilities and some module actions are limited to specific stages.
For example, particle spawning is possible in Emitter Spawn and Emitter Update, but not the other stages. (I assume, haven't tested.)

An Emitter can be made as an [[Asset]], meaning it can be reused multiple times in the same [[Niagara System]], and used by multiple systems.


# Adding An Emitter

A Emitter is either local to a single [[Niagara System]] or an [[Asset]] in the [[Content Browser]] usable by many Systems.
An Emitter is added either when the [[Niagara System]] is created or from the [[Niagara Editor]].
To create a new emitter within the [[Niagara Editor]] right-click the System Overview and select Add Emitter.
Or hit `E` on the keyboard.
A list of templates is presented.
A template is an emitter that comes with a set of modules already added.
All modules added by the template can be removed, it doesn't matter which template you create an emitter from.
(I think.)
Can also add an Emitter with Timeline panel > Track > Emitter > pick a template.

Modules are removed from an Emitter by clicking them to select and then hitting the Del key on the keyboard.

An emitter can be renamed by selecting it in the [[Niagara Editor]] and hitting F2.
And emitter can be renamed by right-clicking it in the [[Niagara Editor]] and selecting Rename.
An emitter can be renamed from the left part of the Timeline panel, double-click the name and type a new one.

An Emitter consists of a stack of [[Niagara Module|Niagara Modules]] grouped into stages.
The groups are
- Emitter Spawn
- Emitter Update
- Particle Spawn
- Particle Update
- Event Handling
- Render

# Customizing An Emitter

When a [[Niagara Emitter]] [[Asset]] is added to a [[Niagara System]]  it becomes a node in the System Overview panel of the [[Niagara Editor]].
The node lists all the [[Niagara Module]]s that the Emitter [[Asset]] specifies, within their respective stages.
We can add additional modules if we want.
The inherited Modules will be marked with a padlock, signifying that they cannot be removed from the inheriting Emitter.
Inherited Modules can be disabled by unchecking the checkbox next to them.
We can configure variables on these Modules to tweak the Emitter's behavior to match our use case, and to make it integrate with the other Modules in the Emitter.
When a Module is selected in the Emitter node the Node's variables are listed in the Selection panel.

We can find the [[Asset]] an Emitter was created from with right-click > Show In Content Browser.
Opening and modifying the Emitter [[Asset]] will modify all instance of that Emitter.

Some Emitter variables, such as Life Cycle Mode and Scalability Mode, can be set to System, which means the Emitter inherits the value set on the [[Niagara System]] that owns a particular Emitter instance.
Set Life Cycle Mode to Self to enable a bunch of settings that would otherwise be inherited, settings such as Inactive Response and Loop Behavior.

## Emitter Properties

- Local Space: Whether particle positions are relative to the owning [[Niagara Component]]'s location or relative to the global world origin.
- Determinism:
- Sim Target: Which processor the Emitter's [[Niagara Module]]s should execute on, either the CPU (CPUSim) or the GPU (GPU Compute Sim).

# Emitter Spawn - Initialize Parameters

What is Emitter Spawn for?

# Emitter Update - Spawning Particles

Particles are spawned by adding one of the many Spawn modules to the Emitter Update Module group.
- Spawn Rate continuously creates new particles at a particular rate.
- Spawn Burst Instantaneous creates a bunch of particles at some point in time.
- You can also create your own custom particle spawning [[Niagara Module]].

After the particles have been spawned the Particle Spawn Module group is run for every new particle.
This is where we initialize the properties of the new particles.
It is common to have a Module that set the Position [[Niagara Attribute]], for example with the Sphere Location module.

# Particle Spawn - Initializing Particles

# Particle Update - Moving Particles

# Event Handling - Communicating With Other Emitters

# Render - Visualize Particles


# Emitter Properties

## Require Persistent IDs

Not sure, something about particles being reordered from time to time.
A guess is that there is a Get Particle / Read Attribute By ID node and if Require Persistent IDs is disabled then we may get a different particle between frames even if the first particle is still alive, but with Require Persistent IDs then we get the same particle throughout the lifetime of that particle.
An implementation may be that Get Particle / Read Attribute By ID does a simple index-based look-up when Require Persistent IDs is disabled and a two-stage look-up in a ID-to-index table when Require Persistent IDs is enabled.


# Emitter Spawning Other Emitters

An Emitter can spawn other Emitters.
This is done by generating a [[Niagara Event]] that some other Emitter listens for.
You can, for example, add the Generate Location Event [[Niagara Module]] to the Particle Update stage of the source emitter.

The receiving Emitter listens for this even by adding a Receive Location Event Module to the Event Handler stage.
Multiple Emitters can listen for the same event.

Events that can be generated include
- Location
- Collision
- Death

# References

- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
- [_Intro To Niagara_ by Epic Online Learning, James Hill @ dev.epicgames.com/tutorials 2023 UE5.2](https://dev.epicgames.com/community/learning/tutorials/8B1P/unreal-engine-intro-to-niagara)
