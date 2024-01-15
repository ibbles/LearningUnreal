The usage of real-world material properties for rendering.
Material properties that can be measured.
An upgrade over older techniques such as [Phong shading](https://en.wikipedia.org/wiki/Phong_shading).
The set of material properties used varies between different software.
A common set, also used by Unreal Engine, is:
- Base color
	- The overall color of the material.
	- Sometimes provided by a [[Texture]] with `Albedo` in the name.
- Metallic
- Specular
- Roughness

These properties are often read from a [[Texture]] in the [[Material]].


# References

- [_Material Editor Fundamentals for Game Development_ > _PBR Properties and the Material Editor_ by Epic Games, Lincoln Hughes @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/pm/unreal-engine-material-editor-fundamentals-for-game-development/PZb/unreal-engine-pbr-properties-and-the-material-editor)
