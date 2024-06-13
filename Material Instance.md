A Material Instance is created in order to efficiently create variations of a [[Master Material]],
which is a regular [[Material]] with one or more [[Material Parameter|Material Parameters]].
The master [[Material]] provides customization points, called [[Material Parameter|Material Parameters]], that the Material Instance can configure.
It is even possible to modify these parameters at runtime using [[Blueprint Visual Script]] or C++.
In most projects almost all references to a [[Material]] is to a Material Instance [[Asset]].

A Material Instance is a [[Material]] that inherits from another [[Material]], possibly via other Material Instances.
In a Material Instance we can set values for the parameters provided by the root Material.
Modifying parameters on a Material Instance is much faster than modifying the root Material,
which isn't even possible at runtime.

A Material Instance doesn't create a new [[Shader]], which makes working with them much faster.
Editing a [[Material Parameter]] from the Material Instance Editor is instantaneous.

If junior developers are restricted to only work on Material Instances there is very little they can mess up.
Senior developers can focus on the small number of [[Master Material|Master Materials]].

A Material Instance is created from the [[Content Browser]] by one of
- Right-click > Materials & Textures > Material Instance.
- Right-click a [[Material]] > Create Material Instance.
A Material Instance [[Asset]] has a darker green color than a [[Material]] [[Asset]] in the [[Content Browser]].


# Material Instance Editor

The Material Instance Editor is a reduced version of the [[Material Editor]].
There is no [[Material Graph]].
The only panels we have are:
- Viewport.
- [[Details Panel]].
- Instance Parents.

The [[Material Parameter|Material Parameters]] inherited from the [[Master Material]] are listed in the [[Details Panel]] under Parameter Groups.
To override an inherited value tick the check-box next to the parameter name and then set a new value.
If the parameter is a [[Texture]] then we can drag one from the [[Content Browser]] to the parameter.
The [[Details Panel]] also lists the parent [[Material]].
Double-click the parent [[Material]] icon to open it.
The parent may also be a Material Instance, but at the root of a Material Instance parent chain is always a [[Material]].

# Material Property Overrides

Some of the "material global" (not sure what word to use here) of the parent [[Material]] can be overridden by a Material Instance.
These settings are found in [[Details Panel]] > General > Material Property Overrides.
You can, for example, override the [[Blend Mode]] and the [[Shading Model]].


# Setting Material Parameter Values At Runtime

To modify a [[Material Parameter]] at runtime we need a [[Dynamic Material Instance]].
We can create one from a [[Static Mesh]] using the Create Dynamic Material Instance function,
which returns a Material Instance Dynamic.
It will also replace the [[Material]] on the [[Static Mesh]] with the newly created Material Instance Dynamic.
Material Instance Dynamic has the Set Scalar Parameter Value function,
which is used to modify [[Material Parameter]] values.
Changing a [[Material Parameter]] through the Material Instance Dynamic will immediately change the look of the [[Static Mesh]],
since the creation of the Material Instance Dynamic also made it the [[Material]] used on the [[Static Mesh]].

# References

- [_Materials Master Learning_ > _Material Instances_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/o6r/material-instances)
- [_Becoming an Environment Artist in Unreal Engine_ > _Material Masters and Instances_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/7Bb/unreal-engine-material-masters-and-instances)



