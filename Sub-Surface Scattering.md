Sub-surface scattering is when light enters a material, bounces around, and then exits the material, picking up the internal color of the object along the way.
This is common in organic materials such as leaves and skin.
Illuminating the object from behind makes it look like the object is glowing.

Applying sub-surface scattering to a [[Mesh]] often includes the usage of a sub-surface scattering texture map.
The sub-surface scattering texture is a single-channel texture indicating where the thickness of the object, i.e. the opposite of where it is translucent, i.e. where the internal color becomes visible when illuminated from behind, and where it is opaque.
It has a value between 0.0 and 1.0. (I assume.) where 0.0 means very thin, i.e. very translucent, and 1.0 means very thick, i.e. very opaque.

Sub-surface scattering is controlled from a [[Material]].
Set [[Details Panel]] > Material > [[Shading Model]] to Subsurface to enable sub-surface scattering.
This will activate the Subsurface Color input pin on the [[Material Output Node]].

The amount of sub-surface color that is visible is controlled by the Opacity input pin on the [[Material Output Node]].
Connect a [[Texture Sample]] reading from the sub-surface texture map to the Opacity input pin.

When the opacity is low then the color passed to the Subsurface Color pin will show when the object is illuminated from behind.

# References

- [_An In-Depth Look at Environment Artist Based Tools_ > Applying Sub-Surface_ by Epic Online Learning @ dev.epicgames.com/courses 2021 UE4.27]
