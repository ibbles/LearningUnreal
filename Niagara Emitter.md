A Niagara Emitter is a piece of a [[Niagara System]].
The emitter is responsible for spawning new particles.
An emitter consists of [[Niagara Module|Niagara Modules]].
The modules define the behavior or the emitter.


# Adding Emitter

An Emitter is added either when the [[Niagara System]] is created or from the [[Niagara Editor]].
To create a new emitter within the [[Niagara Editor]] right-click the System Overview and select Add Emitter.
Or hit `E` on the keyboard.
A list of templates is presented.
A template is an emitter that comes with a set of modules already added.
All modules added by the template can be removed, it doesn't matter which template you create an emitter from.
(I think.)
Can also add an Emitter with Timeline panel > Track > Emitter > pick a template.

Modules are removed from an Emitter by clicking them to select and then hitting the Del key on the keyboard.

An emitter can be renamed from the left part of the Timeline panel, double-click the name and type a new one.

An Emitter consists of a stack of [[Niagara Module|Niagara Modules]] grouped into groups.
The groups are
- Emitter Spawn
- Emitter Update
- Particle Spawn
- Particle Update
- Event Handling
- Render


# Emitter Spawn - Initialize Parameters

What is Emitter Spawn for?

# Emitter Update - Spawning Particles

Particles are spawned by adding one of the many Spawn modules to the Emitter Update Module group.
Spawn Rate continuously creates new particles at a particular rate.
Spawn Burst Instantaneous creates a bunch of particles at some point in time.

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
