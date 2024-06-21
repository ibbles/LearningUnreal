Lumen is Unreal Engine 5's new dynamic global illumination lighting system.
It is a dynamic lighting model, as opposed to the old static lighting model that required light baking.
[[Lightmap|Lightmaps]] are no longer used.
Provides:
- Global illumination.
- Ambient occlusion.
- Reflections.

Enabled from [[Project Settings]] > Engine > Rendering:
- Set Global Illumination > Dynamic Global Illumination Method to Lumen.
- Set Reflections > Reflection Method to Lumen.

Since Lumen is a dynamic lighting model, all [[Light Source|Light Sources]] should have their [[Mobility]] set to Movable.

To ensure that all lighting is using Lumen and that no costly [[Baked Lighting|Light Baking]] happens set [[Project Settings]] > Engine > Rendering > Misc Lighting > Allow Static Lighting to false.

A [[Light Source]] may have a Indirect Lighting Intensity setting in the Details panel.
This setting controls how much that light source contribute to the global illumination around objects directly illuminated by the light source.


# Requirements

On Windows, set Project Settings > Platforms > Windows > Default RHI to DirectX 12,
and enable DirectX 11 & 12 (SM5).

On Linux enable, set Project Setting > Platforms > Linux > Vulkan Desktop (SM5).

In Project Settings > Engine > Rendering
- Set Global Illumination > Dynamic Global Illumination Method to Lumen.
- Enable Lumen > Use Hardware Ray Tracing When Available.
- Set Lumen > Ray Lighting Mode to Surface Cache.
- Set Lumen > Software Ray Tracing Mode to Detail Tracing.
- Set Shadows > Shadow Map Method to Virtual Shadow Maps.
- Enable Hardware Ray Tracing > Support Hardware Ray Tracing.

(
Not sure if the settings above are suitable for real-time rendering or more for cinematics.
)


# Lumen Configuration

Lumen is configured from [[Project Settings]] > Engine > Rendering.
Quality settings can be overridden locally with a [[Post Process Volume]].


## Project Settings

- Global Illumination
	- Dynamic Global Illumination Method: Set to Lumen to use Lumen.
- Reflections
	- Reflection Method: Set to Lumen to use Lumen.
	- Reflection Capture Resolution: 128 for real-time, 2,048 for high-quality non-real-time.
- Lumen
	- User Hardware Ray Tracing When Available:
	- Ray Lighting Mode: Control how reflections work when using hardware ray-tracing. (What is done when not using hardware ray-tracing? Always reflect the Surface Cache?)
		- Surface Cache: Reflections will only show the surface cache, not the actual object being reflected.
		- Hit Lighting For Reflections: Calculate lighting at the ray hit point. High GPU cost since this will run the [[Material Graph]] for reflections. We see the actual object in the reflection instead of the Surface Cache.

## Post Process Volume

These settings are the the [[Details Panel]].

- Global Illumination
	- Method: Switch between None, Lumen, and Screen Space.
	- Lumen Global Illumination
		- Quality settings for Lumen Global Illumination.
	- Ray Tracing Global Illumination
		- Quality settings for Ray Tracing Global Illumination.
- Reflection

# Ray Tracing

Lumen can use both software and hardware ray-tracing.
The default is software ray-tracing.
The central difference is what the light rays are being traced against.
With hardware ray-tracing the light rays are traced against the actual rendered mesh.
This can be very time consuming even when hardware accelerated.
The software mode instead traces against [[Mesh Distance Field|Mesh Distance Fields]].
This is an approximation of the mesh stored as a voxel grid.

The default ray-tracing mode is set in [[Project Settings]] > Engine > Rendering > Lumen > Use Hardware Ray Tracing When Available.
Ray-traced shadows can be controlled per [[Light Source]] at [[Details Panel]] > Light > Advanced > Cast Ray Traced Shadows.
I don't know if there is a way to set whether a particular [[Light Source]] should use software or hardware ray-tracing.

A [[Post Process Volume]] can also be used to control some aspects of the ray-tracing done within the volume.

## Software Ray-Tracing

Software ray-tracing can only render meshes that have a [[Mesh Distance Field]].
For the distance fields to be generated [[Project Settings]] > Engine > Rendering > Software Ray Tracing > Generate Mesh Distance fields must be enabled.
You can set a voxel density to control how closely the voxel grid will approximate the meshes.
Increasing (Or is it decreasing? What is the unit here?) comes with a memory, drive size, and frame time cost.
The value set in the [[Project Settings]] can be overridden per mesh per [[Mesh LOD]] level in the [[Static Mesh Editor]] with [[Details Panel]] > LOD # > Build Settings > Distance Field Resolution Scale.
The distance fields can be visualized in the [[Level Viewport]] by enabling the Visualize > Mesh Distance Fields [[Show]] toggle.
This replaced the regular Static Mesh rendering with their distance fields representation.
This is a good way to identify approximation failures leading to lighting artifacts such as light leakage.

## Hardware Ray-Tracing

Lumen can use hardware ray tracing, if supported by the hardware.
Ray tracing is supported by NVIDIA RTX cards, AMD Radeon 6000 series and up, and Intel ARC.
Hardware ray tracing support for Lumen is enabled with [[Project Settings]] > Engine > Rendering > Lumen > User Hardware Ray Tracing When Available.

Hardware ray-tracing is mainly for mirror reflections and [[Material|Materials]] with [[Translucency]].
Supports [[Skeletal Mesh]] in addition to [[Static Mesh]] because it traces against the actual triangles instead of the surface cache.

With hardware ray tracing enabled Lumen reflections will include off-screen [[Mobility]] = Movable objects.
(
I think mobility is the determining factor.
)
Make sure [[Project Settings]] > Engine > Rendering > Reflections > Reflection Method is set to Lumen.

There are a bunch of other Ray Tracing related settings in [[Project Settings]] > Engine > Rendering > Hardware Ray Tracing.
I don't think they are related to Lumen.
Not sure under what conditions they apply.

On a [[Post Process Volume]] we can tweak some more Lumen settings.


# Surface Cache

Lumen uses a cache to "accumulate light information over time".
(
Not sure if the bit in quotes is true, or if the cache is used for something else.
)
This cache is stored in a set of cards per-mesh.
By default 12 cards per mesh, which can be changed from [[Static Mesh Editor]] > Details > LOD # > Build Settings > Max Lumen Mesh Cards.
If the mesh to too complex then you may need to split it up into multiple pieces.
This is typically true for walls, floor, and ceiling, these should be separate meshes.
The cards are used to evaluate lighting when a ray hit is detected with the mesh.
It is a simplified version of the mesh and its [[Material]] properties from various angles.
The generation of these cards works best when the mesh is a [[Nanite]] mesh, especially when there are lots of small triangles.
[[Foliage]] and [[Instanced Static Mesh]] must use [[Nanite]] in order to get any cards.
The cards can be visualized in the [[Level Viewport]] by running the following console command:
```
r.Lumen.Visualize.CardPlacement 1
```
You can visualize the scene as produced by the cards by enabling [[View Mode]] > Lumen > Surface Cache.
Any area that lack a surface cache, i.e. that doesn't have any cards, will be colored magenta.
The effect of not having a surface cache is that they will not contribute to bounce light and will we black in reflections.


# Lumen Visualization

The Lumen Scene is the world as seen by the light rays.
It can be visualized in the [[Level Viewport]] with [[View Mode]] > Lumen > Lumen Scene.
For the best result you want the Lumen Scene [[View Mode]] to be as similar as possible to the Lit [[View Mode]].
Look for black meshes, that's a sign of problems and Lumen will fall back to screen traces only.
Also check the [[View Mode]] > Lumen > Surface Cache, where areas without surface cache shows up in magenta.
There areas will not produce any light bounce.
Run `r.Lumen.Visualize.CardPlacement 1` in the console to visualize the surface cache cards themselves.


# Post-Process Volume

A [[Post Process Volume]] can control some aspects of Lumen.

## Lumen Scene Detail

Global Illumination > Lumen Global Illumination > Lumen Scene Detail controls the size of meshes that are included in the Lumen scene.
Meshes that are too small are culled away.
I'm not sure what the unit of Lumen Scene Detail is.
Set Lumen Scene Detail larger to include smaller meshes, set it lower to cull away larger meshes.
Uses screen space size, so large meshes are culled away when they are farther away.
Culled meshes are shown in yellow when the `r.Lumen.Visualize.CardPlacement` visualization is enabled.
There can be visible artifacts when meshes are moved in and out of the cull state.

## Lumen Reflections

The highest quality reflections are produced by setting Reflections > Lumen Reflections > :
- Ray Lighting Mode to Hit Lighting For Reflections.
- High Quality Translucency Reflections to On.

# Noise / Flickering / Jittering / Crawling Indirect Lighting

Sometimes Lumen causes flickering / jittering / crawling light in indirectly illuminated parts of the level.
This happens because Lumen is an approximate light propagation simulation algorithm.
We can reduce it by increasing the Lumen quality settings.
In a Post Process Volume, increase Global Illumination > Lumen Global Illumination >
- Lumen Scene Lighting Quality
- Final Gather Quality
- Advanced > Final Gather Lighting Update Speed

One can also edit the `r.Lumen.Reflections.ScreenSpaceReconstruction.TonemapStrength` [[Console Variable]].
This clamps the intensity of reflections which means brightness will go down a bit,
but this is often preferable to the noise.
Try with a value of 1, tweak as necessary.


# References

- [_Lighting in Unreal Engine 5 for Beginners_, by William Faucher @ youtube.com](https://www.youtube.com/watch?v=fSbBsXbjxPo)
- [_Lighting in Unreal Engine 5 for Beginners_ - Fixing crawling indirect lighting, by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=2185)
- [_Unreal Engine 5 New Lumen Hardware Ray tracing Reflections_, by JSFILMZ @ youtube.com](https://www.youtube.com/watch?v=rQ0zJFgdqHE)
- [_LUMEN in Unreal Engine 5 | How it REALLY works ?_ by Proj Prod @ youtube.com 2023](https://www.youtube.com/watch?v=HZFBUhusQn4)
- [_Rough Lumen reflections cheat sheet_ by ](https://forums.unrealengine.com/t/rough-lumen-reflections-cheat-sheet/875826/5)
- [_Introducing Post Processing_ by Epic  Online Learning, Kevin Lyle @ dev.epicgames.com/courses 2023 UE5.0](https://dev.epicgames.com/community/learning/courses/pE2/unreal-engine-introducing-post-processing/mZ11/unreal-engine-introducing-post-processing-overview)
