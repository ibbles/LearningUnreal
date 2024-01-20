The [[Shader|Shaders]] that results from a [[Material]] takes some time to execute on the GPU during a [[Draw Call]], increasing render times and decreasing frame-rates.
How much time depend on:
- The number of pixels being rendered.
- The number of operations / instructions in the [[Pixel Shader]] and the cost of each.
- The number of vertices in the [[Mesh]].
- The number of operations / instructions in the [[Vertex Shader]] and the cost of each.

The [[Pixel Shader]] is often more costly than the [[Vertex Shader]].


# Shader Complexity

You can see how costly the various [[Material|Materials]] in your scene is using [[View Mode]] > Optimization Viewmodes > Shader Complexity.
See also [[Rendering Performance Profiling And Analysis]].
The number of instructions for the shaders can be seen in [[Material Editor]] > [[Stats Panel]].
An [[Expression Node]] is not the same as one instructions.
Some nodes, such as Add, are very cheap.
Some nodes, such as Noise with Function set to Voronoi, is very expensive.
So you cannot estimate the cost of a [[Material]] from the size of the [[Material Graph]].
As a general recommendation, for a PC game, keep the number of instructions at or below a few hundred,
and lower than that for materials that will cover a large part of the screen such as ground, floor, walls, or foliage.
High hundreds if you are targeting high-end hardware.
(
How well does these numbers hold up over time?
Do they increase with faster GPUs?
This recommendation is from [_Materials Master Learning_ > _Performance_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/oJjW/unreal-engine-performance).
)

To reduce the complexity of a [[Material]] that renders some complicated surface pattern in real-time graphics we rarely use procedural materials,
instead we use [[Texture|Textures]].
The [[Material]] may do calculations based on the texture samples, but the base of the material's look-and-feel is defined by the textures.


# Translucency

Translucency is computationally expensive to render so keep the [[Material]] as simple as possible.
Because it is evaluated over and over again, once for every translucent layer covering a pixel.
This is called overdraw and is generally bad for performance.
Because it causes the final image to depend on the sorting order of the translucent surfaces.
Must first render whatever opaque object is behind the translucent objects, then each translucent object in turn closer and closer to the camera to modify the color of the pixel.
The costs of these add up.

If you need [[Blend Mode]] to be [[Translucency|Translucent]] try to keep the [[Shading Model]] set to Unlit.
If it must be Lit, prefer to use Details panel > Translucency > Lighting Mode set to Volumetric NonDirectional.
Will produce blurry received shadows, i.e, shadows on the translucent object from the environment.
Can be made less blurry by increasing the `r.TranslucencyLightingVolumeDim` [[Console Variable]], the default is 64 so try increasing to 256 or so.
The instruction count on Translucent materials is more important than on regular materials, keep it low.

Translucency can be faked by setting the [[Blend Mode]] to Masked and setting Details panel > Material > Dither Opacity Mask.
Will render faster.
Pass in Opacity to the Opacity Mask input pin on the Material Output Node .


# Mask

A binary way to handle transparency in a [[Material]].
A rendered pixel is either on or off, shown or not shown.
We say that some pixels are discarded.
Used for fences, wires, plants, vegetation.
Does not give soft transitions.

Faster to render than translucency, but the off-parts still comes with increased cost since we must render the things seen through the holes and gaps.
The off-pixels are subject to overdraw effects.
For performance, minimize the amount of invisible parts when the [[Mesh]] is rendered.
For example, when rendering grass patches as flat polygons with a masked [[Material]] it makes sense to cut the mesh to follow the rough outline of the grass patch to avoid large costly off-areas at the top corners.
Avoid needless overdraw.


# Final Notes And Performance Tips

Ways to get more performance:
- Feature level switch.
	- One input per supported [[Shader Model]].
	- Only the input associated with the currently active [[Shader Model]] is evaluated.
	- So that a cheaper, lower-quality version can be used for less powerful hardware.
- Quality switch.
	- An end-user configurable setting.
	- Input pins for Low, Medium, and High.
	- Use a cheaper, lower-quality version for the lower-quality settings.
- Fully rough.
	- A property in the [[Material|Material's]] [[Details Panel]].
- Shader permutations.
- Texture group sizes.

Forward + reflections is very expensive.
Not sure what this means.
By "forward" does he mean not the deferred rendering pipeline?
By "reflections", does he mean high specular and low roughness?
[Rendering Kickstart - Rendering - Advice @ learn.unrealengine.com](https://learn.unrealengine.com/course/3537481/module/6853946)


See also [[Rendering Performance Profiling And Analysis]].


# References

- [_Materials Master Learning_ > _Performance_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/oJjW/unreal-engine-performance)

