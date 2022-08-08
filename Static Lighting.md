Static Lighting is also called Baked Lighting.
Lighting that is calculated at build-time by a tool built into Unreal Engine named [[Lightmass]].
More limited than [[Dynamic Lighting]] but also more performant at runtime and higher quality.
Supports [[Global Illumination]].

At runtime it doesn't matter how many static lights you have to illuminate an object, all that light is precomputed into the [[Lightmap]] for that object and after that only the texture matter, how much computation went into creating that texture is irrelevant.
Doing the baking, i.e. running [[Lightmass]], will take more time if there are more light sources.
It also matters what resolution we use for each object's [[Lightmap]].
High-resolution lightmaps will be more computationally heavy both during baking and rendering.

Setting up Static Lighting is more complicated than [[Dynamic Lighting]], there are more settings to get right and more of a process to go through.
[[Dynamic Lighting]] is also typically easier to use during level design since we can see the end result immediately.
Static Lighting must be built before we can see the final results, so there is a lot of guessing going on.
We can see a preview of the static lighting, but it will change, potentially drastically, once baking has finished.
We can recognize preview lighting by the Preview label that is printed on the shadows.

Must build lighting once with Top Menu Bar > Build > Build Lighting Only to update everything.
What has been built is what will be rendered and it is therefore not possible to change the light at runtime.

Static lighting can be disabled completely with [[World Settings]] > Lightmass > Advanced > Force No Precomputed Lighting.

A [[Light Source]] produces Static Lighting if its [[Mobility]] ([[Light Mobility]]) is set to Static or Stationary.
Setting the [[Mobility]] to Movable turns the light into a dynamic light.

The scene lighting needs to be rebuilt every time a static or stationary light or object is changed in the scene.
If there are at least one static light in the scene and the scene is modified within that lights influence volume then a message saying "LIGHTING NEEDS TO BE REBUILT" is typically printed to the  top-right corner of the viewport.


# Lighting Quality

Unreal Engine can build lighting at different levels of quality.
These are controlled in Build > Lighting Quality.

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



