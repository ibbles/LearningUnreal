A property of [[Material]], just like [[Shading Model]].
Control how meshes rendered with this material is blended with the surroundings.
Used for [[Translucency]].
A [[Material Instance]] can override the parent [[Material|Material's]] Blend Mode from [[Details Panel]] > General > Material Property Overrides > Blend Mode.

# Opaque
All pixels from this mesh completely overwrite whatever is behind it.

# Masked

Enables the Opacity Mask input pin on the [[Material Output Node]].
We can discard parts of the object per pixel.
Included pixels are opaque.
Excluded pixels are completely invisible.
A 1-bit opacity, either visible or not visible.
Used to mask out complicated shapes from very few triangles.
Commonly used for foliage, to trace out the shape of leaves.
Often used with an opacity mask, which is a texture with an alpha channel that is either 0 or 1.

Pixels that are included are rendered just like they would have with the Opaque blend mode.

The parts that are discarded are still rendered.
So shape your mesh to minimize the amount of discarded areas, they are cost for no gain.
(
According to [_Materials Master Learning_ > _Blend Mode - Part One](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/EV7/blend-mode-part-one) the discarded parts are even more expensive than the non-discarded parts.
Not sure why that is.
Because the depth test for all meshes at that pixel can't work as efficiently?
)

Despite the above caveat, masking is a very fast way of doing transparency and should be used over Translucent wherever possible.

## Dithered Translucent

A variant of the Masked blend mode is a technique called dithered translucent.
Not a separate blend mode and not actually translucency.
Uses a regular Masked blend mode [[Material]] and uses the Dither Temporal AA material node connected to the Opacity Mask input pin.
Will cause alternating, or some other repeated pattern of, pixels to be either visible or invisible.
Sort of like looking through a mesh.
If the resolution is high enough then the individual pixels won't be visible and it surface looks translucent.

Has reflections, but also some ghosting.
Doesn't work well for large flat surfaces such as glass windows.
May work well for things in the distance.

# Translucent

This blend mode enables the Opacity input pin on the [[Material Output Node]].
It also disables the Metallic, Specular, Roughness, and Normal input pins,
unless we enable [[Details Panel]] > Translucency > Screen Space Reflections,
and set Translucency > Lighting Mode to Surface Forward Shading.
This is very computationally expensive.

We control per-pixel transparency.

Can be combined with the Unlit and Default Lit  [[Shading Model|Shading Models]].
With Default Lit we pass the the final color to Base Color.
With Unlit we pass the final color to Emissive Color.
Default Lit and Unlit look very similar in many circumstances for translucent materials.
Unlit is computationally cheaper than Default Lit.

In a deferred renderer, see [[Rendering Pipeline]], it is difficult to make translucent surfaces that:
- display reflections
- interacts with lighting and shading

This means that Unlit translucent surfaces looks similar to Default Lit translucent surfaces but are much faster to compute.
It is therefore recommended to use Unlit translucent surfaces over Default Lit translucent surfaces in most cases.

If you need high-quality translucent surfaces then, in the [[Details Panel]], enable Screen Space Reflections and set Lighting Mode to Surface Forward Shading.
This is very computationally expensive.

See also [[Translucency]].


# Additive

Adds the color of the rendered object on top of whatever was rendered behind it.

# Modulate

Removes the bright bits, only keeps the darker pixels.
Useful for stains, dirt, blast marks from an explosion.

# Alpha Composite

# References

- [_Materials Master Learning_ > _Blend Mode - Part One_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/EV7/blend-mode-part-one)