Allows one [[Niagara Emitter]] to read Attributes of another Emitter, or itself.
An ID is used to read Attributes of a particular particle of the Emitter.
Each particle has an Attribute named `UniqueID`.
Referenced as `Particles.UniqueID`.
This ID is not actually completely unique.
The Get ID At Spawn Index produces an ID that is actually unique.
One can also enable Requires Persistent IDs on the [[Niagara Emitter]], then the `UniqueID` will be really unique.
(
I think, there is some confusion on which ID is mean each time the word ID is used in []().
)


# Reading An Attribute Of A Particle
The Attribute Reader is a parameter on the Module and should be bound to an Attribute Reader variable on the [[Niagara System]].
The Attribute Reader variable should be bound to the [[Niagara Emitter]] that the Attribute Reader should read from.
Inside the Module use e.g. a Get Vector By ID node to read a particular Attribute.
Input to Get Vector By ID is the name of the Attribute to read, the Attribute Reader to identify the Emitter to read particle data from, and a Particle ID to identify which particle to read the Attribute for, e.g. which particle's position we are interested in.
There is also Get ... By ID.

The Niagara runtime will detect write-read dependencies between Emitters and run Modules for the Emitter that contains the write before the modules in the Emitter that contains the read.
Not sure it if can handle broken updates, i.e. if it can switch back and forth between emitters, ping-ponging data between them.


# References

- [_The Art of Illusion - Niagara Simulation Framework Overview_ - Particle Attribute Reader by Asher Zhu @ youtube.com 2020](https://youtu.be/-cKgZrrBJ2w?t=764)


