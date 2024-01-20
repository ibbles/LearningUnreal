The usage of real-world material properties for rendering,
along with an approximation of what light actually does in the real world.
Material properties that can be measured.
An upgrade over older techniques such as [Phong shading](https://en.wikipedia.org/wiki/Phong_shading).
Supposed to work well in all lighting environments,
something older shading models had problems with.
Used for most things that are rendered in Unreal.
Advantages:
- By using physically based rendering for most things a lot of development focus can be put on that part of the pipeline.
- Improves art pipeline by being predictable.
- Works with GBuffer, see [[Rendering Pipeline]], based deferred rendering and compositing based workflow.
- Can use known standard parameter values for many materials.

The set of material properties used varies between different software.
A common set, also used by Unreal Engine, is:
- Base color
	- The overall color of the material.
	- Sometimes provided by a [[Texture]] with `Albedo` in the name.
- Metallic
- Specular
- Roughness

These properties are often read from a [[Texture]] in the [[Material]].

Some example values:
```
MATERIAL    Specular
Glass       0.5
Plastic     0.5
Quartz      0.57
Ice         0.224
Water       0.255
Milk        0.277
Skin        0.35
```

# References

- [_Material Editor Fundamentals for Game Development_ > _PBR Properties and the Material Editor_ by Epic Games, Lincoln Hughes @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/pm/unreal-engine-material-editor-fundamentals-for-game-development/PZb/unreal-engine-pbr-properties-and-the-material-editor)
- [_Materials Master Learning_ > _Physically Based Rendering_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/oJjW/unreal-engine-performance)

