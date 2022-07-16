Translucency means that a material is partially transparent, it has a non-binary opacity.
It can also have a refraction index, meaning that it distorts the direction of light passing through it.
Challenging to to in real time.
Hardware [[Ray Tracing]] helps in the high quality / low frame rate use-cases.
Unreal Engine has many variants.


# Material Details Panel Settings

Enabled on a [[Material]] by setting Details panel > Material > Blend Mode to Translucent.
Prefer to set Details panel > Material > Shading Model to Unlit, for performance.
Otherwise, if you need lit translucency set Shading Model to Default Lit.


## Screen Space Reflections

Adds accurate reflections to the object.
Looks good but computationally expensive.
(
Text missing here, where is that setting?
)


## Lighting Mode

Under Details panel > Translucency.
Affects both performance and render quality.

- Volumetric NonDirectional
  - The cheapest one.
- Volumetrix PerVertex NonDirectional
    - Similar to Volumetric NonDirectional.
- Surface Forward Shading
  - Most computationally costly but best looking.


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
When set to one there is no refraction, light rays passes straight through the object.
Increasing it causes more and more refraction.
The index of refraction of glass is 1.5.


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

