A Material Parameter is a  value that is configurable from the outside of the [[Material]].
We call a [[Material]] that has at least one Material Parameter a [[Master Material]].
The parameter is created in a [[Material]] and is available, and can be overridden, in and [[Material Instance]] that has that [[Material]] as its root parent.
Each [[Material Instance]] created from  a [[Material]] can have its own value for the each parameter in the [[Master Material]].

It is recommended to set the default values so that they feature they control is disabled, if possible.
A parameter that brightens to texture should brighten nothing by default.
A parameter that tints a texture should tint by nothing by default.

A Material Parameters can be
- scalar.
- vector.
- texture sample.
	- 2D.
	- Cube.
	- SubUV.
- texture object
- static bool
- font sample.
- component mask
- static switch

Modifying a Material Parameter in a Material Instance is very fast, much faster than edit the same value in the parent material.

Create a new parameter by
- right-clicking any Constant, Constant#Vector, or Texture Sample node and select Convert To Parameter.
- creating a Make * node, right-click > Convert To Parameter.
- right-click an input pin > Promote To Parameter.
- Holding `s` and clicking to get a scalar parameter.
	- Is there a button to hold for a 2,3,4 component vector parameter?

Set a name for the parameter either in the node itself or at [[Details Panel]] > General > Parameter name.

Values nodes that represent a Material Parameter has the word Param, along with the default value, under the node title.

In the [[Material Editor]] for a [[Material Instance]] the parameters we created in the parent material are listed in the [[Details Panel]] under Parameter Groups.
Each parameter can optionally be overridden.
Check the check-box next to a parameter name to override it.
The value-column becomes editable and we can set the value we want this [[Material Instance]] to have for that [[Material Parameter]].
Values not overridden will use the value set in the parent material.


# Dynamic Material Instance

A [[Dynamic Material Instance]] is a [[Material Instance]] on which we can modify the Material Parameters at runtime.
A [[Dynamic Material Instance]] can be created from a [[Static Mesh Component]] using the Create Dynamic Material Instance functions.
Create Dynamic Material Instance returns a Material Instance Dynamics which has the function Set Scalar Parameter Value.
An others for other Material Parameter types.
Set Scalar Parameter Value has two parameters:
- Parameter Name: The name of the Material Parameter to set a new value for, e.g. "Roughness".
- Value: The new value for the Material Parameter, e.g. 0.5.


# Static Switch Parameter

Used to let a [[Material Instance]] turn parts of the [[Material Graph]] on or off.
The node is titled Switch Param.
The Static Switch node has a True input pin and a False input pin.
If the Static Switch Parameter is set to True then only the True branch is evaluated.
If the Static Switch Parameter is set to False then only the False branch is evaluated.


# Groups

Parameters can be grouped.
This puts them in a named category within the [[Details Panel]] when editing a [[Material Instance]]
Assign a group by selecting the parameter node and edit Details panel > Material Expression > Group.
By setting a Sort Priority we decide where within that group a particular parameter is placed.


# Material Parameter Collection

A global, project-wide, Material Parameter that is accessible from all [[Material|Materials]].
A way to modify the same Material Parameter in many [[Material Instance|Material Instances]] at once.
A Material Parameter Collection is an [[Asset]].
It contains a list of scalar parameters and a list of vector parameters.
Each parameter has a name and a value.

To use a Material Parameter Collection in a [[Material]] add a Collection Parameter node.
The Collection Parameter node has Collection Param underneath the title.
In the [[Details Panel]], the node has a reference to the Material Parameter Collection [[Asset]],
and a Parameter Name to identify which parameter in the collection this particular node should output.

A [[Material]] can only use two Material Parameter Collections.
One strategy is to have a general collection that any [[Material]] can use and a bunch of more specific ones that are used in a [[Material]] on a case-by-case basis.
The general collection can contain things like the time of day, color of the sky, and similar.

A Material Parameter Collection cannot have more than 1024 parameters.

# References

- [_Material Editor Fundamentals for Game Development_ > Master Materials, Material Instances, and Parameters_ by Epic Games, Lincoln Hughes @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/pm/unreal-engine-material-editor-fundamentals-for-game-development/b6Z/unreal-engine-master-materials-material-instances-and-parameters)
- [_Materials Master Learning_ > _Material Instances_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/o6r/material-instances)
- [_Materials Master Learning_ > _Material Parameter Collections_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/qLO/material-parameter-collections)
- [_Becoming an Environment Artist in Unreal Engine_ > _Material Masters and Instances_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/7Bb/unreal-engine-material-masters-and-instances)


