Outdoor environments are often illuminated by a [[Directional Light]] simulating sun light.
The sun light [[Light Source]] often has its [[Light Mobility]] set to stationary so that it get both baked [[Global Illumination]] and high-quality dynamic shadows computed with [[Cascaded Shadow Maps]] and optionally [[Distance Field Shadows]].
If you are using [[Lumen]] and [[Virtual Shadow Maps]] then I'm not sure what the [[Light Mobility]] should be set to,
or what the trade-offs are for the three options.


# Shadows

In an outdoor environment it is common to have long view distances.
This implies both a high number of shadows and far away shadows.
This is computationally expensive when using [[Dynamic Lighting]].
So we need a [[LOD]] system for the shadows.
This [[LOD]] system is a combination of [[Cascaded Shadow Maps]] and [[Distance Field Shadows]].
[[Cascaded Shadow Maps]] are a [[LOD]] system in and of themselves.
They are a sequence of [[Shadow Maps]] with lower and lower resolution, kinda like [[Texture]] mip levels.
The farther the object is from the camera, the lower resolution the shadow will be.
There is a fixed number of shadow map levels and a distance at which the last shadow map runs out.
At that point the object's shadow can either disappear or be replaced by [[Distance Field Shadows]].
[[Distance Field Shadows]] don't look at the [[Mesh]] itself when generating the shadow, but instead look at a fixed resolution and static distance field representation of the mesh.
This shadowing method is much faster than triangle-based shadowing methods, but doesn't support animation.
The shadow will not move when the object is animated.
If the object moves, i.e. its Location changes, then the shadow will follow.
But an animated [[Skeletal Mesh]] or a [[Static Mesh]] with a material using [[World Position Offset]] will not have an animated shadow.



# References

- [_Lighting Essential Concepts and Effects - Dynamic Lighting - Outdoor_, by Epic Games @ dev.epicgames.com, 2018](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/V0M/dynamic-lighting-outdoor)

