A Niagara Module is a piece of code that implements some part of the functionality of a [[Niagara System]].
A Module is an [[Asset]] that lives in the [[Content Browser]].
It is created with [[Content Browser]] > Niagara > Niagara Module Script.
A modules is added to a [[Niagara Emitter]].
We say that the Emitter calls the Module which causes the module to be executed, i.e. its code is run.
Modules are executed at emitter spawn, particle spawn, emitter update, particle update, or render depending on where in the Emitter it is placed.

Module calls can be copy-pasted between Emitters.
Selecting an existing Module and then Ctrl+V to paste a copied Module will insert the Module call after the selected Module.


# Input

A Module can have input parameters.
These show up in the Selection panel when a Module is selected in an Emitter.
The parameter can be bound to user parameters, see [[Niagara System]].
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
(
I think. Or is this only for [[Niagara Attributes]]?
)


# Output

A Module can write to the particle's [[Niagara Attribute|Attributes]].
This is done with the Map Set node.
By connecting any value wire to the Map Set node we create a new local variable of that type.
By default new variables are placed in the Local [[Niagara Namespace]].
We can convert the local variable to a Module output by right-click the Map Set input pin > Change Namespace > Output.
This will cause the value to ... [[TODO]] (What does output do?)

To change the name of the value in the Map Set node double-click the name and type the new name.


# Troubleshooting

## Compiler error: `Cannot set external constant`

Happens when there is a user parameter, in the Parameters tab, with the same name as a built-in parameter.
In my case it was `User.NumParticles` that conflicted with `Engine.Emitter.NumParticles`.
Renamed the user parameter to `TargetNumParticles` and the error went away.