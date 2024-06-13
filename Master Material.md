A Master Material is a [[Material]] that is intended to be instanced.
It provides a number of [[Material Parameter|Material Parameters]] that a [[Material Instance]] can override.
It is common that a project has a small set of master materials and a much larger set of [[Material Instance|Material Instances]].
In this way we only need to implement shared functionality once and reuse for many materials.
There is often one Master Material per type of surface, for example
- Stone
- Vegetation
- Glass

Different looks of these are created by creating a [[Material Instance]] for each look.
It can be difficult to know when a new feature should be implemented as a [[Material Parameter]] or a new Master Material.
Adding functionality to a Master Material will add it to all [[Material Instance]] with that Master Material as its parent.
That may or may not be what you want.
A common mistake is to make too large and complicated Master Materials,
which may come with unnecessary performance cost,
or the shader permutation cost to skyrocket because a large number of [[Material Parameter|static switches]] has been used,
which causes massive amounts of recompilation every time the Master Material is modified.
There is a balance between too few and too many Master Materials.
Expect between 20-50 Master Materials in a large project.

A Master Material is rarely used directly,
in most cases all usages are with a [[Material Instance]].

Tip: Put a single [[Material Instance]] between the Master Material and all other Material Instances using that Master Material.
In this way it is quick and easy to make global parameter changes of the default values for everything.
# References

- [_Material Editor Fundamentals for Game Development_ > Master Materials, Material Instances, and Parameters_ by Epic Games, Lincoln Hughes @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/pm/unreal-engine-material-editor-fundamentals-for-game-development/b6Z/unreal-engine-master-materials-material-instances-and-parameters)
- [_Materials Master Learning_ > _Material Instances_ by Epic Games @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/o6r/material-instances)
- [_Becoming an Environment Artist in Unreal Engine_ > _Material Masters and Instances_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/7Bb/unreal-engine-material-masters-and-instances)

