A pixel shader is a GPU program that is run as part of a [[Draw Call]] per-pixel.
A pixel shader can read and write GPU buffers and have access to data associated with the [[Mesh]] and triangle currently being rendered.
The pixel shader defines what the shader does, the functionality, but not what it does it on and it often leaves a number of values unspecified.
The "what it does it on" part is controlled from Unreal Editor when we assign the corresponding [[Material]] to a mesh and bind [[Texture|Textures]], [[Render Target|Render Targets]], etc in a [[Material Instance]].
The unspecified values are [[Material Parameter|Material Parameters]] that are also specified in a [[Material Instance]].
So for a pixel shader to run we must specify:
- The [[Material]] that was used to create the shader.
- A [[Texture]] for each [[Texture Sample]] used by the [[Material]].
- A value for each [[Material Parameter]].
- A [[Mesh]] to render.

The [[Material]] assigned to a [[Mesh]] define, at least in part, what the pixel shader will do when the [[Mesh]] is rendered.
Unreal adds a bunch of extra shader code, called [[Shader Template|Shader Templates]] needed by the [[Rendering Pipeline]].
So the actual shader program is a large HLSL program Epic Games designed that contains customization points that a we configure per [[Material]] through the [[Material Editor]].
Each input pin on the [[Material Output Node]] is one of those customization points.
The settings set in the [[Details Panel]] of the [[Material]] defines which engine-provided shader code is included and which customization points the [[Material]] must specify, i.e. which input pins are active on the [[Material Output Node]].
Settings such as Material Domain, Blend Mode, and Shading Model. (Anything else?)
The shader code is available in the engine installation, in the `Engine/Shaders` directory.

Additional HLSL code from the engine [[Shader Template|Shader Templates]] may be added based on usage.
For example, if the [[Material]] is assigned to a [[Static Mesh Component]] with [[Mobility]] set to Static and that has a precomputed [[Lightmap]] then a [[Shader Template]] for using that [[Lightmap]] will be tacked on to the shader code when rendering that mesh, but not when rendering a Movable [[Static Mesh Component]].
Another example is the Usage > Use With X checkboxes in the [[Material|Material's]] [[Details Panel]], each checkbox leads to a new [[Shader Template]] and thus a new shader variant.
These checkboxes may be ticked automatically based on how we use the [[Material]] in our [[Level]].
There are many other settings and circumstances than necessitates changes  to the shader code and this leads to a combinatorically increasing number of permutations to compile.
This is why we sometimes see the engine compiling why more shaders than we have [[Material|Materials]].
Examples of settings causing or reducing shader permutations:
- Quality Switch node.
- Static Switch node.
- Feature Level Switch node.
- Target platforms in the [[Project Settings]].
- Lighting settings in the [[Project Settings]].
- Shader Permutation Reduction in the [[Project Settings]].
	- (I think it is in the [[Project Settings]].)

The final result of a render is a combination of the expression network in the [[Material]] and the [[Shader Template|Shader Templates]] included in the engine.

You can see the generated HLSL code for a [[Material]] with [[Material Editor]] > Top Menu Bar > Window > HLSL Code.

Different platforms uses different shading languages, i.e. the language in which we tell the GPU what the shader program should do.
DirectX uses the High-Level Shading Language, a.k.a. HLSL.
Unreal Engine uses HLSL when generating code for a [[Material]] and then cross compiles that for language used on other platforms.
Other platforms include Android and Vulkan.

See also [[Vertex Shader]].

# References

- [_Materials Master Learning_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/bVy/introduction)
- [_Materials Master Learning_ > _Material Input - Part One_by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/KzX/material-inputs-part-one)
