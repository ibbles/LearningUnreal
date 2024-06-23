A Niagara Module is a piece of code that implements some part of the functionality of a [[Niagara System]].
A Module is an [[Asset]] that lives in the [[Content Browser]].
It is created with [[Content Browser]] > Niagara > Niagara Module Script.
A modules is added to a [[Niagara Emitter]].
We say that the Emitter calls the Module which causes the module to be executed, i.e. its code is run.
Modules are executed at emitter spawn, particle spawn, emitter update, particle update, or render depending on where in the Emitter it is placed.

Module calls can be copy-pasted between Emitters.
Select a module in one Emitter and hit Ctrl+C to copy it.
Selecting an existing Module in another Emitter and then Ctrl+V to paste the copied Module into the selected Emitter.
The copied Module will be inserted after the selected Module.


# Input

## Parameters

A Module can have input parameters.
These show up in the Selection panel when a Module is selected in an Emitter.
The parameter can be bound to a user parameter, see [[Niagara System]].
This is how users of a Niagara System can tweak the particle simulation.
They can also be bound to a [[Niagara Attribute]].
Then the module will read the Attribute for the particle that the Module is currently executed for.
In the Particle Spawn group the module is executed for each new particle.
In the Particle Update group the module is executed for every particle in the System.
Not sure what happens when the Module is part of a [[Niagara Simulation Stage]] whose domain is a [[Niagara Data Interface]] instead of the particles.

The parameter can be a [[Blueprint Structure]], if the structure has been registered at [[Project Settings]] > Plugins > Niagara > Additional Parameter Types.
(
I think, haven't tested yet.
)

The parameters are accessed in the module with a Map Get node.
The parameters are within the INPUT [[Niagara Namespace]].

Create a new parameter by clicking the + button next to an empty output pinon a Map Get node, expand Make New and select a type for the new parameter.
You can change the [[Niagara Namespace]] of the parameter by right-click > Change Namespace > pick a namespace.

Some parameters are not read with a Map Get node, but instead used by a Static Switch node.

## Input Map - Map Get

A Module can read data stored per-particle, per-emitter, or per-system (Not sure if all three are possible.) using a Map Get node.

A Module may contain multiple Map Get nodes, making it easier to split the work up into multiple phases within the [[Node Graph Editor]].
Each phase can have its own Map Get node that reads the data relevant for that phase.


# Output Map - Map Set

A Module can write to the particle's [[Niagara Attribute|Attributes]].
This is done with the Map Set node.
By connecting any value wire to the Map Set node we create a new local variable of that type.
By default new variables are placed in the Local [[Niagara Namespace]].
We can convert the local variable to a Module output by right-click the Map Set input pin > Change Namespace > Output.
This will cause the value to ... [[TODO]] (What does output do?)

We can click the + button next to an empty input pin to specify where a value connected to that pin should be written.
Particle attributes are within the PARTICLES [[Niagara Namespace]].


To change the name of the value in the Map Set node double-click the name and type the new name.


# Troubleshooting

## Compiler error: `Cannot set external constant`

Happens when there is a user parameter, in the Parameters tab, with the same name as a built-in parameter.
In my case it was `User.NumParticles` that conflicted with `Engine.Emitter.NumParticles`.
Renamed the user parameter to `TargetNumParticles` and the error went away.

# References

- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
