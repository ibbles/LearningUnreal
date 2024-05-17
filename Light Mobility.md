I don't know what in this note changes when using [[Lumen]] instead of the legacy lighting engine.
(
Does the non-Lumen lighting engine have a name?
)

The [[Mobility]] of a light source control whether the light is [[Static Lighting|Static]] or [[Dynamic Lighting|Dynamic]].
Static lighting is faster at runtime but require baking and can't be changed at runtime.
Dynamic lighting is more computationally demanding at runtime but doesn't require baking and can be changed at runtime.
There is also a hybrid blend between static and dynamic, tries to be the best of both worlds.
A scene can contain [[Light Source|Light Sources]] in any combination of the three.

It is not possible to change the [[Mobility]] of a [[Light Source]] at runtime.

# Movable

When a [[Light]]'s mobility is set to Movable then its light is fully dynamic, both direct and indirect lighting.
Indirect lighting, also called [[Global Illumination]], is only supported with [[Lumen]], not with the legacy lighting system.
Without [[Global Illumination]] any part of the level that is in shadow from all lights will be completely dark,
except possibly for [[Sky Light]].
It is the most expensive to render.

Dynamic light is computed at runtime.
This includes [[Shadows]] and [[Shading]].
Produces very sharp and well-defined shadows.
Maybe too sharp, unless [[Ray Traced Shadows]] or [[Distance Field Shadows]] are used.
Very large objects that cast very long shadows may get precision errors when viewed from a distance.
Don't know in what way those precision errors manifests.
There is no [[Lightmap]] involved, so [[Mesh|Meshes]] don't need Lightmap UVs.

Increasing the number of dynamic light sources will increase the computational cost of the scene dramatically.
Especially if they have overlapping attenuation radius, see [[Light Source Properties]].
The number of meshes within a light's attenuation radius also influence the runtime cost.
The cost of a dynamic light can be reduced significantly by disabling Details panel > Light > Cast Shadows.
[[Dynamic Shadows]] is one of the most computationally expensive things in the engine.

A movable light is required if the light will be moved or if parameters other than color or intensity is changed at runtime.
If the light will stay still by may change color or intensity then its mobility can be set to stationary.

It will not influence baked lighting such as [[Lightmass]].
It will cast [[Dynamic Shadows]] if Details panel > Light > Cast Shadows is enabled.
If at least one of Cast Dynamic Shadows and Cast Static Shadows is enabled in the same category.

Uses a different [[Shadows|Shadows]] method than stationary lights.


# Stationary

A hybrid blend between movable and static, trying to have a bit of the advantages of both while minimizing the drawbacks of them.
Not as fast to render as static lights, but faster than movable lights.
Means that the light won't move, but there are some properties, such as intensity and color, that can change at runtime.
Not sure if there are other parameters that can change as well.
Will result in partially baked lighting.
Direct lighting and direct shadows will be computed at runtime.
Indirect lighting, also called [[Global Illumination]], will be baked.

This means that we get dynamic shadows that support both [[Skeletal Mesh]] and [[World Position Offset]] animation
using [[Cascaded Shadow Maps]] and optionally [[Distance Field Shadows]], or [[Virtual Shadow Maps]] if that has been enabled in the project.

Increasing the number of stationary lights will come with a computation cost both during baking and at runtime.

A stationary light will bake its shadows into a [[Lightmap]].
All stationary light sources share the same [[Lightmap]] texture and that texture only has four channels.
That means that you can only have four stationary lights illuminating the same objects.
When there is too many then some of them will get a red cross in the [[Level Viewport]].
Fix this by either moving the lights farther apart or by reducing the attenuation radius, see [[Light Source Properties]].
The attenuation radius fix is a guess from my side, haven't tested yet.
The limitation is only for shadows, so the problem can also be fixed by disabling Details panel > Light > Cast Shadows on some of the light sources.
Or switch the light to Static or Movable, since the four shadow channel limitation only applies to stationary light sources.

Uses a different [[Shadows|Shadows]] method than movable lights.

Sunlight is often set to stationary.


# Static

The [[Light]] may not change in any way during runtime.
Can be fully baked using [[Lightmass]], producing [[Baked Lighting]] with a [[Lightmap]] for each static [[Static Mesh]] and Volumetric Lightmap for everything else.
Produces both direct and indirect lighting.
It is the cheapest to render.

Shadows can become blurry or blocky since they are limited to the [[Lightmap]] resolution of the illuminated mesh, and also depending on the [[Lightmass]] settings.
For high-quality shadows consider changing to stationary,
which gives you both [[Global Illumination]] and high-quality dynamics animated shadows,
at the cost of reduced runtime performance.

Increasing the number of static light sources will increase the time it takes to bake the lighting,
but will not increase the computational cost at runtime.
The light from all light sources is baked into the same set of textures regardless of how many lights there are.

Whenever something is changed in the scene that affects the baked [[Lightmap|Lightmaps]] a Preview text will be shown on the objects that receive baked lighting.
This means that what is shown in the [[Level Viewport]] isn't representative of what the final package project will look like.
If you move a light that has already been baked then you will see both the baked lighting and the preview lighting on top of each other,
visible as a double-shadow effect.

# References
- [_Lighting in Unreal Engine 5 for Beginners_ by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=318)
- [_Understanding Lightmass - Baking Checklist_, by Epic Games @ dev.epicgames.com](https://dev.epicgames.com/community/learning/courses/yon/introducing-global-illumination/kn8/understanding-lightmass-baking-checklist)
- [_Lighting Essential Concepts and Effects - Dynamic Lighting - Indoor and Basics_, by Epic Games @ dev.epicgames.com, 2018](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/mX9k/dynamic-lighting-indoor-and-basics)
- [_Becoming an Environment Artist in Unreal Engine_ > _Lighting_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE 4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/oE6/unreal-engine-lighting)



