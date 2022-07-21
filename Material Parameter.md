A material parameter is a  value that is configurable from the outside of the [[Material]].
The parameter is created in a [[Material]] and is then available in [[Material Instance|Material Instances]].
Each [[Material Instance]] created from  a [[Material]] can have its own value for the parameters.

Parameters can be scalars, vectors, textures, ...
(
Anything else?
)

Create a new parameter by
- creating a Make ??? node, right-click > Convert To Parameter.
- right-click an input pin > Promote To Parameter.

Parameters can grouped.
Assign a group at Details panel > Material Expression > Group.
By setting a Sort Priority we decide where within that group a particular parameter is placed.
