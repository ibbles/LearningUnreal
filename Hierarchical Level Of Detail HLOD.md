HLOD for short.
A type of [[Mesh LOD]] where multiple [[Static Mesh]]es are rendered as a single mesh when far away. (Not always true? There are different HLOD types.)
That mesh forms a proxy for all the individual meshes.
Works with [[World Partition]] in that when a grid cell is unloaded then it is replaced with the HLODs that exists in that area of the [[Level]].
Typically one proxy mesh per HLOD Layer per grid cell.

Display the HLOD cells that are loaded with the [[Console Command]]
```
wp.Runtime.ToggleDrawRuntimeHash2D
```


Process the entire world to build, or bake, a new version of geometry and textures in the distance as an [[Instanced Static Mesh]].
Make it possible to have those far-away objects visible without the memory and performance overhead of rendering them individually using their real assets.


# HLOD Layer

The build process is controlled using HLOD Layers.
An HLOD Layer is an asset created with [[Content Browser]] > Miscellaneous > HLOD  Layer.
You can have multiple HLOD Layers in a [[Level]].

Each HLOD Layer has a Layer Type:
- Instancing: [[Instanced Static Mesh]] using the lowest quality [[Mesh LOD]].
- Merged Mesh: Take everything and merge it all down to a single [[Static Mesh]].
- Simplified Mesh: Create a brand new set of geometry and [[Material]]s
- Approximated Mesh: The same as Simplified Mesh, but made for [[Nanite]] meshes.
- Custom:

## Instancing

[[Static Mesh]]es are replaced with [[Instanced Static Mesh]]es using the lowest [[Mesh LOD]].
Good for trees and foliage.

(
[Mentioned (39:20)](https://dev.epicgames.com/community/learning/talks-and-demos/jwlJ/unreal-engine-building-bigger-changing-your-workflow-for-building-worlds-instead-of-scenes-unreal-fest-2023) that the mesh needs to be flagged as "instanced".
I don't know what this means, where to flag it, or what other effects setting this flag will have.
)

## Merged Mesh

Meshes are merged to create a single proxy mesh.
Creates a single [[Material]] that is a simplified version of all [[Material]]s used.

## Simplified Mesh

Same as Merged Mesh, but also runs a mesh simplification pass to reduce the number of triangles.

## /

What other settings are available depend on what Layer Type is set to.
- Simplified Mesh.
	- Proxy Settings: Settings for how complex mesh is generated.
	- Material Settings: What material setup to use. E.g if a roughness map should be generated or if a constant roughness is good enough.
(
I have no idea how to use any of the above.
)

Can have different HLOD Layers for different kinds of things:
- Foliage.
- Roofs.
- Walls.
- River stones.


# Building HLODs

The build process is started with [[Top Menu Bar]] > Build > _World Partition_ > Build HLODs.
Can take a long time.
Will only rebuild invalidated HLODs, so future builds will be faster as long as you didn't change everything.
Is multi-threaded.
Can be run as a [[Commandlet]].
```
$UE_EDITOR \
	MyProject.uproject \
	-Run=WorldPartitionBuilderCommandlet \
	-AllowCommandletRendering \
	-Builder=WorldPartitionHLODsBuilder \
	[-SetupHLODS | -BuildHLODS | -DeleteHLODs]
```

The generated HLOD [[Actor]]s will be placed in the [[Outliner]] in a folder named [[HLOD]].
(I think.)
There may be sub-folders.

# References

- [_Building Open Worlds in Unreal Engine 5 | Unreal Fest 2022_ by Epic Online Learning @ dev.epicgames.com/talks-and-demos](https://dev.epicgames.com/community/learning/talks-and-demos/LLM5/building-open-worlds-in-unreal-engine-5-unreal-fest-2022)
- [_Building Bigger: Changing Your Workflow for Building Worlds instead of Scenes | Unreal Fest 2023_ by Chris Murphy @ dev.epicgames.com/talks-and-demos 2024 UE5.3](https://dev.epicgames.com/community/learning/talks-and-demos/jwlJ/unreal-engine-building-bigger-changing-your-workflow-for-building-worlds-instead-of-scenes-unreal-fest-2023)



