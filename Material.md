A Material defines the look-and-feel of an objects.
Unreal Engine uses [[Physically Based Rendering]], so the purpose of a Material is to define values for the parameters used by that rendering model.
See _Main Material Node_ below, [[Material Editor]], [[Material Graph]] and [[Rendering Pipeline]].

The function of a Material is defined by its [[Material Graph]], which is constructed in the [[Material Editor]].
A Material has a bunch of inputs.
Some, such as texture coordinates and vertex colors, come from the mesh being rendered.
Some, such as camera position and time, come from the engine.
Some, the [[Material Parameter|Material Parameters]], are user provided.
The author of the Material creates the parameters and implement their semantics through the [[Material Graph]].
See [[Material Input]].

Material is a type of [[Asset]], new Materials are created from the [[Content Browser]].
Right-click in the Content Browser and select Material.
To assign a Material to a [[Static Mesh]]
- drag it from the [[Content Browser]] and drop it on the [[Static Mesh]] in the [[Level Viewport]]
- assign it to one of the Material > Element # slots in the Static Mesh's [[Details Panel]].

There will be as many Material > Element # entries in the Static Mesh's [[Details Panel]] as there are [[Material Slot|Material Slots]] in the [[Static Mesh]].

A Material can be used in different contexts, called a [[Material Domain]].
Example domains include Surface, Light Function, and Post Process Blendable.


# Parameters And Material Instances

A Material can have [[Material Parameter|Material Parameters]] and we can create a [[Material Instance]] or a [[Dynamic Material Instance]] that set these parameters to particular values.
Parameters are created by adding a Constant node, created by holding 1 to 4 and clicking in the Material Graph, right-click the Constant node and select Convert To Parameter.
Can also be created by right-clicking any input pin on any node and select Promote To Parameter.
Give the parameter a name and a default value.


# Main Material Node

The main part of a Material is the main Material node, also called the [[Material Output Node]].
The values passed to the input pins of the output node define the function of the Material.
Depending on the settings set in the [[Details Panel]] different input pins are available.
Settings that effect the pins available include [[Blend Mode]] and [[Shading Model]].
See also [[Physically Based Rendering]].

Default values are shown in () below.

- Base Color:
	- The color of the material.
	- Red, green, and blue each in the range 0.0..1.0. Larger values are clamped.
- Metallic (0.0):
	- How metallic the material is. I don't know what this means.
	- Increasing it reduces the influence of Base Color, makes reflections more visible.
	- Many recommend using this as a boolean, i.e. only ever pass 0.0 or 1.0. I don't know why.
- Specular (0.5):
	- In the majority of cases this can be left unconnected, using the default value.
	- Setting Specular to 0.0 removes reflections.
	- Does nothing if Metallic is 1.0.
- Roughness (1.0):
	- Values closer to 0.0 makes it more smooth and mirror-like, values closer to 1.0 makes it more diffuse.
	- 1.0 means that light bounces off it in all directions.
- Anisotropy:
- Emissive Color (0.0, 0.0, 0.0):
	- Causes the material to illuminating, to glow. Controls bloom.
	- Can accept color values larger than 1.0.
	- Does not actually emit light, does not shine on surrounding objects and does not cause shadows.
		- With [[Lumen]] I think it does. (Test it.)
	- Used for small lights, like LED diodes, fire,
- Opacity:
- Opacity Mask:
- Normal
- Tangent:
- World Position Offset:
	- Evaluated by the [[Vertex Shader]].
	- Used to tweak the position of the vertex.
	- Does not create new vertices, so needs a high-resolution mesh for high-resolution surface details.
	- Can be used to create procedural wind animation for foliage.
		- Create a grass texture where the vertices at the bottom have black [[Vertex Color]] and the ones at the top has [[Vertex Color]] color, with a gradient between them.
		-  Provide two 3-component vector values, one negative and one positive, to control the amount of sway of the grass.
		- Use the Time node, pass it through a Sine node, offset and scale to 0.0..1.0 range, pass to a [[Lerp]] node lerping between the two sway amount values, multiply the resulting sway amount with the green channel of the [[Vertex Color]], connect to the World Position Offset input pin.
		- The vertices at the bottom will not sway since they are black so the last multiply will multiply by 0.0.
		- The vertices at the top will sway by the set sway amount since they are multiplied by 1.0, the all-green color read from the [[Vertex Color]] for those vertices.
		- The vertices in-between will sway some portion of the sway amount depending on their height and thus amount of green in the [[Vertex Color]].
	- Can be used to create wave-like patterns.
		- Use a pair of Panner nodes, with different settings, to pan a grayscale noise texture twice over the surface, multiply the two samples together, scale to wanted height, append with 0.0 and 0.0 to produce a (0.0, 0.0, noise\*noise) vector, connect to World Position Offset.
	- Can be used to create uneven surfaces from a [[Height Map]].
		- Sample a [[Height Map]] [[Texture]], multiply that with a scalar to control offset distance, multiply by the Vertex Normal WS to move in the correct direction, and connect to the World Position Offset input pin.
- Subsurface Color:
- Custom Data 0:
- Custom Data 1:
- Ambient Occlusion:
	- Not sure exactly.
	- Does not work with dynamics lights.
		- (As of Unreal Engine 4.something, 2019. Does it work with [[Lumen]]?)
- Refraction:
- Pixel Depth Offset:
	- Offsets the location of the pixels.
	- Not sure in which direction. Away from the camera?
	- Used for making grass on a plane appear more natural when it intersects with a flat ground.
	- By using a grayscale texture painting some blades of grass white, some gray, and some dark gray with get some variation of where they become occluded by the ground.
	- There is no straight intersection line anymore.
	- The same can be done with hair.
	- Can be used in combination with the Parallax Displacement Map.
		- If we lower a parallaxed flat plane down towards a flat ground the grooves won't disappear first.
		- The entire parallaxed object will remain visible until the plane passes through the ground, at which point the entire object disappears all at once.
		- The illusion of depth is lost.
		- By adding a Pixel Depth Offset that is larger in the grooves we push those further away into the ground and those pixels a Z-culled away before the higher parts of the [[Height Map]].
- Shading Model:


If you are modeling a real-world material It is sometimes possible to find suitable ranges for many of these values on the internet.
For example "PBR roughness plastic".
See also [[Physically Based Rendering]]


## Base Color: Vector

The inherent, light-independent, color that the object has.
The color the object would have if viewed under pure white completely diffused all-encompassing light.
Sometimes called "albedo".
Not sure if this is a linear or sRGB color.


## Normal: Vector

Control which direction the surface is facing in [[Tangent Space]] by default, so not [[World Space]] or [[Model Space]].
[[Tangent Space]] means that the Z axis is pointing out of the triangle, no matter how the triangle is placed or rotated.
So passing a full-blue color produces the "natural" normal for the mesh.
By passing other colors that vary over the surface we can create the illusion of structure unevenness smaller than a single triangle.
Often read from a texture called a normal map.

We can switch the Normal input pin to take a [[World Space]] normal instead by disabling Details panel > Material > Advanced > Tangent Space Normal.

We can convert between spaces with the Transform Vector node, called Vector Ops > Transform in the list.
The Transform Vector node can convert
- Tangent Space
- Local Space
- World Space
- View Space
- Camera Space
- Mesh Particle Space

I assume the normal should be normalized.
Or perhaps the next step of the pipeline will always normalize it?


## Tangent: Vector


# Details panel
The following properties are available in the Details panel of the [[Material Editor]].

## Material
- Blend Mode
	- Control how this Material with interact with other objects being rendered behind it.
	- **Opaque** means that this material will not blend, the pixel value is used as-is regardless of what's behind it.
	- **Masked** means that a pixel will either be opaque or discarded. Used to creates shapes smaller than the triangles. Common with foliage.
	- **Translucent**: We get gradual opacity, can set it 0..1 per pixel.
- Advanced > Fully Rough: boolean
	- Max out roughness, remove any glossiness from the materials.
	- Simplifies the generated [[Shader]], removing parts of the [[Physically Based Rendering]], which improves performance.
		- Not sure by how much.
	- Not sure how this relates to the Roughness output, which is still enabled even after setting Fully Rough to true.
		- The pin value seems to be ignored.
- Use Material Attributes
	- Collapse all the input pins on the [[Material Output Node]] to a single pin. This is useful when you are passing entire material setups around through [[Material Function|Material Functions]], mixing layers and such.
- Dithered LOD Transition: Make the transition between LOD levels smoother.
	- This might be for [[Foliage]] rendering only, not sure.
	- Can be overridden in a [[Material Instance]].


# References

- [_Material Editor Fundamentals for Game Development_ > PBR Properties and the Material Editor by Epic Games, Lincoln Hughes @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/pm/unreal-engine-material-editor-fundamentals-for-game-development/PZb/unreal-engine-pbr-properties-and-the-material-editor)
- [_Materials Master Learning_ > _Performance_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/oJjW/unreal-engine-performance)
- [_Materials Master Learning_ > _Material Inputs - Part Two_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/9zP/material-inputs-part-two)
- [_Becoming an Environment Artist in Unreal Engine_ > _Basic Material Creation and Application_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/Ya6/unreal-engine-basic-material-creation-and-application)
- [_Becoming an Environment Artist in Unreal Engine_ > _Material Masters and Instances_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/7Bb/unreal-engine-material-masters-and-instances)
