An Attribute is a property of a particle spawned by a [[Niagara Emitter]].
We can view an emitter as a container for buffers and each buffer has one element for each particle emitted by that emitter.
The values at a particular index in all buffers form the state that the particle carries.
Examples of common attributes are `Position` and `Velocity`.
The buffers are namespaced with `Particles` so that we can identify particle attributes among all other variables in a [[Niagara System]].
They are referenced in a [[Niagara Module]] with the Map Get and Map Set nodes.
Whenevera Map Set node has an input pin named `(PARTICLES) Position` then it means that the Module writes to the Position attribute of the current particle.
Modules are typically executed per-particle, i.e. each execution are associated with a particular particle.

It is possible to read Attributes for other particles as well.
This is done with a [[Niagara Particle Attribute Reader]] in a [[Niagara Simulation Stage|Niagara Simulation Stage]].
The Attribute Reader is a parameter on the Module and should be bound to an Attribute Reader variable on the [[Niagara System]].
The Attribute Reader variable should be bound to the [[Niagara Emitter]] that the Attribute Reader should read from.
Inside the Module use e.g. a Get Vector By ID node to read a particular Attribute.
Input to Get Vector By ID is the name of the Attribute to read, the Attribute Reader to identify the Emitter to read particle data from, and a Particle ID to identify which particle to read the Attribute for, e.g. which particle's position we are interested in.
There is also Get ... By ID.