Foliage is a way to add many instances of a collection of [[Static Mesh|Static Meshes]] easily.
Foliage can be added with
- [[Foliage Mode]].
- [[Landscape Foliage]].
- [[Procedural Foliage Volume]].


# Quality Improvements

To make a realistic looking scene it if often necessary to add [[Foliage Wind]].

The LOD transition can be made smoother by enabling Dithered LOD Transition in the Foliage's [[Material]].


# Collisions

Foliage typically does not have collisions enabled.
Foliage created with a Grass node in a [[Landscape Material]] can't have collisions at all.
Foliage created through painting in the [[Foliage Mode]] can have collisions.
To enable, set [[Static Mesh Foliage]] > Details panel > Instance Settings > Collision Presets to Block All.
There are other [[Collision Settings]] as well.


# Warnings

## The Total Lightmap Size Of This InstancedStaticMeshComponent Is Large

This can appear in the Message Log panel when [[Building Lighting]] for a level with foliage.
The [[Lightmap]] size for foliage is reduced in [[Foliage Mode]] > Foliage panel > select one or more [[Foliage Type]]s > Details > Instance Settings > Light Map Resolution.
By enabling the override checkbox and reducing the value we reduce the [[Lightmap]] size for our foliage instances.
This should fix the warning, possibly improve performance, and reduce memory usage.

[_Use Fix and optimize foliage in unreal_ - Light Map Resolution, by Batnobie X @ youtube.com. 2021](https://youtu.be/jcZ5V8qFwgE?t=100)


# References

- [_UE5 Hints & Tips - Foliage and Trees_, by UnrealityBits @ youtube.com](https://www.youtube.com/watch?v=dxofebT02VI)


