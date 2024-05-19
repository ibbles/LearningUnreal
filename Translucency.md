Translucency means that a [[Material]] is partially transparent, it has a non-binary opacity.
It can also have a refraction index, meaning that it distorts the direction of light passing through it.
Challenging to do in real time, it requires a lot of computation time to do well.
Cheaper techniques exists as well, resulting in lower quality.
Hardware [[Ray Tracing]] helps in the high quality / low frame rate use-cases.
Unreal Engine has many variants.
Controlled by choosing a [[Blend Mode]] for a [[Material]].
Translucency is enabled on a [[Material]] by setting [[Material Editor]] > [[Details Panel]] > Material > Blend Mode to Translucent.
Prefer to set [[Details Panel]] > Material > Shading Model to Unlit, for performance.
Otherwise, if you need lit translucency set Shading Model to Default Lit.

Rule of thumb, see below for details:
- If it can be Masked then make it masked.
- If it must be Translucent, try Unlit over Default Lit.
- If it must be lit, prefer a Non-Directional Lighting Mode and keep Screen Space Reflections off.
- If it must be high-quality Surface Forward Shading with Screen Space Reflections then minimize the use of the material and minimize the number of translucent objects that can be seen through each other.
- More translucent surfaces implies that a simpler translucency model should be used.

Translucent objects cannot be selected in the [[Level Viewport]] by default.
Toggle translucent object selection on and off with the T key.


# Material Details Panel Settings

This section is about the various settings in [[Material Editor]] > [[Details Panel]] > Translucency.

## Screen Space Reflections

Enable [[Details Panel]] > Translucency > Screen Space Reflections to get reflections to the surface.
Looks good but computationally expensive.

## Lighting Mode

Under Details panel > Translucency > Lighting Mode.
Affects both performance and render quality.

- Volumetric Non-Directional
  - The cheapest one.
  - The default.
  - Sub-divides the object being rendered into tiles.
  - Lighting calculation is performed per tile and then interpolated over the surface.
  - The tile size is controlled with the `r.TranslucencyLightingVolumeDim` [[Console Variable]].
	  - Smaller numbers decreases the quality, i.e. makes the tiles larger.
	  - 32 is really low.
	  - 128 is kinda high but still with visible artifacts in some cases.
- Volumetric Per Vertex Non-Directional
    - Similar to Volumetric Non-Directional.
    - Lighting calculations are done per-vertex and then interpolated over the triangles.
    - Does not give correct light intensity drop-off with distance.
    - Can make the triangle structure visible.
    - Lighting can disappear completely if there is no vertex within the [[Light Source]] influence radius.
    - Cheap to render.
- Surface Translucent Volume
  - Fairly expensive.
  - Fairly good looking.
- Surface Forward Shading
  - Most computationally costly but best looking.
  - Computes each light per-pixel.
  - Side-steps the deferred renderer for objects using this [[Material]] and uses a forward renderer instead.

Different lighting mode algorithms have different inputs,
which manifests as different input pins on the final material output node.

## Fog, Depth Of Field, Depth Test

Controls how the translucent material interacts with distance fog.
Disable Apply Fogging to make fog not be rendered on the surface at all.
Enabling Compute Fog Per Pixel changes the fog calculation to be done per pixel instead of per vertex and interpolated over the triangles.
Per vertex is usually good enough, but for large surfaces that stretches into the distance, such as bodies of water, per vertex for may make the triangles visible.

If depth of field isn't applied, or is applied when it shouldn't, to the translucent surface then try to enable or disable Render After DOF.

Disable Depth Test can be enabled to make the translucent surface render even when there is an opaque object in front of it.
The translucent object will be rendered in front of everything.
This is non-physical, but useful for icons, markers, and other in-world UI / HUD elements.


# Material Output

## Base Color

A black Base Color makes a glass ball materials less shiny.


## Specular

In Unreal Engine 4.26 and earlier Specular controlled refraction.
In 4.27 and newer we instead have a dedicated Refraction [[Material Output Node]] pin when the Blend Mode is set to Translucent.
(
The last bit is a guess.
)
Specular is now only for reflective highlights, just like for a regular [[Material]].
Set to 0.0 for no specular highlights and 1.0 for the strongest highlights.
1.0 is usually good for glass.


## Roughness

Keep a low value for glass, somewhere between 0.0 and 0.1.


## Opacity

Setting Opacity to 0.0 will make the specular highlights disappear.
Setting Opacity to 1.0 will make the translucent material completely opaque, which isn't really translucent anymore.
For a glass material we want a low but non-zero Opacity,
0.1, 0.2, something like that.


## Refraction

New in Unreal Engine 4.27.
The full name is Index Of Refraction.
When set to 1.0 there is no refraction, light rays passes straight through the object.
Increasing it causes more and more refraction.
The index of refraction of
- Air is 1.0.
- Ice is 1.31
- Water is 1.33.
- Glass is 1.52.
- Diamond is 2.42.

Causes things seen through this object to be distorted.

Only available when the [[Material]]'s [[Blend Mode]] has been set to some form of translucency, such as Translucent.

For example, to produce a curved glass-like effect:
- Use a Fresnel node to get a high value around the edges of an object and low values in the interior.
- Clamp between 0.0 and 1.0.
- Pass to a [[Lerp]] that returns 1.0 for low values, the interior, and a non-1.0 value for high values, the edges.

The refraction calculation will take the fragment's normal into account.
This makes it possible to create refraction effects finely detailed surface patterns, finer than the mesh triangles.

Refraction by itself doesn't work well for flat surfaces such as a pool of water.
For this case you can change [[Details Panel]] > Refraction > Refraction Mode from Index Of Refraction to Pixel Normal Offset.
In some cases one of them is better, in other cases the other one is better. Experiment.

# Post Process Volume

We can control some of the quality settings of the translucency rendering with a [[Post Process Volume]].
In the Post Process Volume's Details panel, under Rendering Features, we have
- Ray Tracing Reflections > Include Translucent Obj... which can be either enabled or disabled.
- Translucency > Type, which can be either Raster or Ray Tracing.
	- Ray Tracing can have much higher quality, but can get really computationally expensive.
- Ray Tracing Translucency.
	- Settings that come into effect when Translucency > Type is set to Ray Tracing.
	- Max Roughness:
	- Max. Refraction Rays: The number of rays of something. Not sure if bounces or branching factor. Increasing this may be necessary to avoid black blotches on translucent objects with a complex shape.
	- Samples Per Pixel:
	- Shadows: Not sure of this is shadows from, onto, or within translucent objects. Seems to be shadows seen through the translucent object.
		- Disabled: No shadows. This may actually look the best sometimes. Eliminates black blotches in translucent objects. Very situational specific.
		- Hard Shadows:
		- Area Shadows:
	- Refraction:

# References

- [_The Awesome 4.27/UE5 Update Nobody Is Talking About_, by William Faucher @ youtube.com](https://www.youtube.com/watch?v=p8TnqBiWKyY)
- [_Materials Master Learning_ > _Blend Mode - Part One_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/EV7/blend-mode-part-one)
