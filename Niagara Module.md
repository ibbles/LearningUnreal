A Niagara Module is a piece of code that implements some part of the functionality of a [[Niagara System]].
A Module is an [[Asset]] that lives in the [[Content Browser]].
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

The parameters are accessed in the module with a Map Get node.
(
I think. Or is this only for [[Niagara Attributes]]?
)


# Output

A Module can write to the particle's [[Niagara Attribute|Attributes]].
This is done with the Map Set node.
