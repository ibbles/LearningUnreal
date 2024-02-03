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
- right-clicking any Constant or Constant#Vector node and select Convert To Parameter.
- creating a Make * node, right-click > Convert To Parameter.
- right-click an input pin > Promote To Parameter.
- Holding `s` and clicking to get a scalar parameter.
	- Is there a button to hold for a 2,3,4 component vector parameter?

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
The Static Switch node has a True input pin and a False input pin.
If the Static Switch Parameter is set to True then only the True branch is evaluated.
If the Static Switch Parameter is set to False then only the False branch is evaluated.

# Groups

Parameters can be grouped.
Assign a group by selecting the parameter node and edit Details panel > Material Expression > Group.
By setting a Sort Priority we decide where within that group a particular parameter is placed.


# References

- [_Material Editor Fundamentals for Game Development_ > Master Materials, Material Instances, and Parameters_ by Epic Games, Lincoln Hughes @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/pm/unreal-engine-material-editor-fundamentals-for-game-development/b6Z/unreal-engine-master-materials-material-instances-and-parameters)
- [_Materials Master Learning_ > _Material Instances_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/o6r/material-instances)

