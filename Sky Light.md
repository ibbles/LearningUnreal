A [[Light]] that produces light from a dome around the scene based on a sky texture.
It produces ambient lighting, but is more advanced than an ambient tint.
For example, during a sunset we can get orange light from one side and sky blue light from the other.
It simulates the atmospheric scattering from the sky.

This texture may be called an HDRI, High Dynamic Range Image, because it has more light information than a regular 8-bit texture.
The sky texture can either be provided or created from the contents of the scene.
Either
- Set Details panel > Light > Source Type to Specified Cubemap and a texture to Details panel > Light > Cubemap.
- Set Details panel > Light > Source Type to Captured Scene.
or enable Details panel > Real Time Capture.

By default the Sky Sphere only uses the upper hemisphere of the sky, i.e. any surface that has a normal pointing down won't get any light.
This is often what you want since there is often a ground blocking any sky light coming from below.
But if you are rendering something high in the sky, away from the ground, then you may want to disable Details panel > Light > Advanced > Lower Hemisphere Is Solid Color.
By default a Sky Light only uses the upper hemisphere of the HDRI texture, it is a SKY light after all.
We can enable the lower hemisphere as well with Sky Light > Details panel > Light > Advanced > Lower Hemisphere Is Solid Color > Disable.

The Sky Light can be turned on and off by toggling [[Details Panel]] > Light > Affects World.
I don't know if there is anything that is considered to be separate from the world,
i.e. that would be receiving light even when Affects World is disabled.

A Sky Light with [[Mobility]] set to Static will only render after [[Baked Lighting|Baking]].
Not sure, but I assume that when using [[Lumen]] the Sky Light is always active.

The Sky Light does not render the texture, you won't see the sky, but it will light objects in the scene.
It will also provide reflections in objects such as chrome balls, when using the [[Lumen]] lighting engine.

It doesn't matter where the Sky Light is placed, it will affect everything.

There is a Light Color property, but you should only rarely need to change that, the cubemap, either specified or captured, handles the color.

You can use the [[HDRI Backdrop]] Actor, which is a combination of a Sky Light and a sky dome mesh, to get both the light and the sky.

A Sky Light with its [[Light Mobility]] set to movable will create [[Distance Field Ambient Occlusion]].

# References

- [_Lighting in Unreal Engine 5 for Beginners_ - Sky Light by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=1284)
- [_Demystifying the Skylight Unreal Engine 4 & 5_ by William Faucher @ youtube.com](https://www.youtube.com/watch?v=BGoaPyfZlYg)
- [_Lighting Essential Concepts and Effects_, Lighting Basics - by Epic Games @ dev.epicgames.com](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/W0K/lighting-basics)
- [_Becoming an Environment Artist in Unreal Engine_ > _Lighting_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE 4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/oE6/unreal-engine-lighting)

