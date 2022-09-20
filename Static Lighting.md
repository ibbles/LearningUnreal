Static Lighting is also called Baked Lighting.
Lighting that is calculated at build-time by a tool built into Unreal Engine named [[Lightmass]].
Static lights can not be changed at runtime, once baked it's fixed.
This means that it only works when both the [[Light Source]] and the [[Mesh]] is non-movable, i.e. [[Mobility]] is either Static or Stationary.
If either the [[Light Source]] or the [[Mesh]] has [[Mobility]] set to Movable then we will only get [[Dynamic Lighting]].
The light baking process can take a long time.
Dynamic objects cannot receive static lighting.
(I assume.)
Static lighting is more limited than [[Dynamic Lighting]] but also more performant at runtime and higher quality.
Supports [[Global Illumination]].
For the best possible lighting quality used static lighting.

[[Lumen]], a dynamic lighting method implemented in Unreal Engine 5.0, make the trade-off between static and dynamic lighting less clear-cut.
[[Lumen]] provide many of the advantages that static lighting provides without baking and with fully dynamic lights.


# Runtime Performance

At runtime it doesn't matter how many static lights you have illuminating an object, all that light is precomputed into the [[Lightmap]] for that object and after that only the texture remain, how much computation went into creating that texture is irrelevant.
Doing the baking, i.e. running [[Lightmass]], will take more time if there are more light sources.

The resolution we use for each object's [[Lightmap]] does impact runtime performance.
High-resolution lightmaps will be more computationally heavy both during baking and rendering.


# Setup

Setting up Static Lighting is more complicated than [[Dynamic Lighting]], there are more settings to get right and more of a process to go through.
[[Dynamic Lighting]] is also typically easier to use during level design since we can see the end result immediately.
Static Lighting must be built before we can see the final results, so there is a lot of guessing going on.
We can see a preview of the static lighting, but it will change, potentially drastically, once baking has finished.
If we have baked the lighting once but later change something in the scene that affects the [[Lightmap|Lightmaps]] then a red message telling us that lighting must be rebuilt is displayed in the upper-left corner of the [[Level Viewport]].
We can recognize preview lighting by the Preview label that is printed on the shadows.

Must build lighting once with Top Menu Bar > Build > Build Lighting Only to update everything.
What has been built is what will be rendered and it is therefore not possible to change the light at runtime.

Static lighting can be disabled completely with [[World Settings]] > Lightmass > Advanced > Force No Precomputed Lighting.

A [[Light Source]] produces Static Lighting if its [[Mobility]] ([[Light Mobility]]) is set to Static or Stationary.
Setting the [[Mobility]] to Movable turns the light into a dynamic light.

The scene lighting needs to be rebuilt every time a static or stationary light or object is changed in the scene.
If there are at least one static light in the scene and the scene is modified within that lights influence volume then a message saying "LIGHTING NEEDS TO BE REBUILT" is typically printed to the  top-right corner of the viewport.

A recommendation is to disable [[Ambient Occlusion]] by setting [[Post Process Volume]] > Rendering Features > Ambient Occlusion > Intensity to 0.0.
This is because the baked lighting get [[Ambient Occlusion]] "for free".
Then also having it be part of the regular rendering pipeline kind of doubles the effect, making corners very dark.

See [[Lightmass]] for more details.


# Material Color

The color, and in particular the brightness (Magnitude of the color values, not talking about emissive here.), of the objects (walls, floor, furniture, etc) and materials in the scene have a big influence on static lighting.
If the objects are dark then you will get very little indirect lighting since so much of the light is absorbed by the objects.
So use bright materials if you want a lot of indirect lighting.
A trick is to temporarily up the brightness of the materials while baking and then bring them back down again.
Then you will get an nonphysical amount of bounce light, but if that's what you want then that's one way of getting it.
Another is to increase the Indirect Lighting Intensity on your [[Post Process Volume]], but if the amount of light in the [[Lightmap]] is really low then this will lead to posterization since we are in effect upscaling a low resolution signal.


# Troubleshooting

A few things to check if static lighting isn't working the way you expect.
- Make sure the [[Light Source]] has its [[Light Mobility]] set to Static or Stationary.
- Make sure the [[Mesh]] has its [[Mobility]] set to Static or Stationary.
- Make sure [[Project Settings]] > Engine > Rendering > Global Illumination Method is set to None.
- Make sure [[Project Settings]] > Engine > Rendering > Misc Lighting > Allow Static Lighting is enabled.
- Make sure [[Project Settings]] > Engine Rendering > Virtual Textures > Enable Virtual Texture Support is (Disabled)|(Enabled).
	- I'm a bit confused here. Some say that Virtual Texturing should be enabled and activated for [[Lightmap|Lightmaps]] while other say that enabling Virtual Texturing broke light building. I'm not sure what to believe.
- Make sure [[World Settings]] > Lightmass > Force No Precomputed Lighting is disabled.
- Make sure [[Post Process Volume]] > Global Illumination > Advanced > Indirect Lighting Intensity is set high enough.



# Lighting Quality

Unreal Engine can build lighting at different levels of quality.
These are controlled in Build > Lighting Quality.
Range from Preview to Production.
No idea what these actually do.

There are also even more settings, independent from the Build > Lighting Quality setting, in the [[World Settings]] > Lightmass > Lightmass Settings group.
The most important ones are
- Static Lighting Level Scale: Overall quality of the lighting, but inverse. Higher number means lower quality.
- Num Indirect Lighting Bounces: The number of times light from "regular" light sources bounce.
- Num Sky Lighting Bounces: The number of times light from [[Sky Light]] bounces.
- Indirect Lighting Quality: The main quality slider. No idea what this does.

The lighting quality also depend on the [[Lightmap]] resolution used for each object.

We can also influence the quality using Lightmass Importance Volume and Lightmass Portal.
See [[Lightmass]] for more information on those.

## References

- [_Lighting in Unreal Engine 5 for Beginners_ by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=309)
- [_Lighting Essential Concepts and Effects_ - Baked Lighting - Lightmass, by Epic Games @ dev.epicgames.com, UE-4.27](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/229/baked-lighting-lightmass)
- [_Bake Lighting FASTER with GPU Lightmass - Unreal Engine 4.26_ by William Faucher @ youtube.com](https://youtu.be/hq1WFFF6iD0)
- [_Lighting with GPU Lightmass | Tips & Tricks | Unreal Engine_ by Epic Games @ youtube.com](https://www.youtube.com/watch?v=RBY82TSLjFA)
- [_Baked global illumination in UE5_, forum thread @ forums.unrealengine.com](https://forums.unrealengine.com/t/baked-global-illumination-in-ue5/514487/13)


