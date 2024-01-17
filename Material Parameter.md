A material parameter is a  value that is configurable from the outside of the [[Material]].
The parameter is created in a [[Material]] and is then available in [[Material Instance|Material Instances]].
Each [[Material Instance]] created from  a [[Material]] can have its own value for the parameters.
It is recommended to set the default values so that they feature they control is disabled, if possible.
A parameter that brightens to texture should brighten nothing by default.
A parameter that tints a texture should tint by nothing by default.

Parameters can be scalars, vectors, textures, ...
(
Anything else?
)

Modifying a material parameter in a material instance is very fast, much faster than edit the same value in the parent material.

Create a new parameter by
- right-clicking any Constant or Constant#Vector node and select Convert To Parameter.
- creating a Make * node, right-click > Convert To Parameter.
- right-click an input pin > Promote To Parameter.
- Holding `s` and clicking to get a scalar parameter.
	- Is there a button to hold for a 2,3,4 component vector parameter?

In the [[Material Editor]] for a [[Material Instance]] the parameters we created in the parent material are listed in the [[Details Panel]] under Parameter Groups.
Each parameter can optionally be overridden.
Check the check-box next to a parameter name to override it.
The value-column becomes editable and we can set the value we want this [[Material Instance]] to have for that [[Material Parameter]].
Values not overridden will use the value set in the parent material.

Parameters can be grouped.
Assign a group at Details panel > Material Expression > Group.
By setting a Sort Priority we decide where within that group a particular parameter is placed.


# References

- [_Material Editor Fundamentals for Game Development_ > Master Materials, Material Instances, and Parameters_ by Epic Games, Lincoln Hughes @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/pm/unreal-engine-material-editor-fundamentals-for-game-development/b6Z/unreal-engine-master-materials-material-instances-and-parameters)

