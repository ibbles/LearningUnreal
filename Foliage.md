Foliage is a way to add many instances of a collection of [[Static Mesh|Static Meshes]] easily.
Foliage can be added with
- [[Foliage Mode]].
- [[Landscape Foliage]].
- [[Procedural Foliage Volume]].


# Quality Improvements

To make a realistic looking scene it if often necessary to add [[Foliage Wind]].

The LOD transition can be made smoother by enabling Dithered LOD Transition in the Foliage's [[Material]].
With Unreal Engine 5.1 we got some support for [[Nanite Foliage]].
[Unreal Engine 5.1 Release Notes - Nanite Improvements](https://docs.unrealengine.com/5.1/en-US/unreal-engine-5.1-release-notes/#naniteimprovements)


# Rendering Artifacts

## Smeary / trailing pixels in swaying branches

I believe this is caused by temporal upsampling, i.e. when the renderer tries to improve the image by reusing data from previous frames.
If the renderer can't track how objects have moved then it will get confused and produce ghostly after-images from objects that has since moved.
It is especially visible on tree branches swaying in the wind.

Possible ways to mitigate:
- Change anti-aliasing mode from Temporal Super Resolution (TSR) to some other method such as FXAA or MSAA.
- Change [[Mobility]] of all problematic Actors from Static to Movable.
- Change [[Project Settings]] > Rendering > Optimizations >
	- Velocity Pass to Write During Base Pass.
		- [_Motion Blur Setting Fix Unreal Engine 5.1_ by Glass Hand Studios @ youtube.com. 2023](https://www.youtube.com/watch?v=WjA4hBLUivQ)
	- Output Velocities Due To Vertex Deformation to Enabled.
		- [_WPO issue in UE 5.0 on all materials, severe smear/dither_ @ forums.unrealengine.com](https://forums.unrealengine.com/t/wpo-issue-in-ue-5-0-on-all-materials-severe-smear-dither/515780)


Write During Depth Pass (default):
![[Images/Smeary_foliage_depth_pass.png]]

Write During Base Pass (Slower?):
![[Images/Smeary_foliage_base_pass.png]]

With Word Position Offset pin disconnected on the Master Material:
![[Images/Smeary_foliage_no_wpo.png]]

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


# Console Variables

There are a few [[Console Variable|Console Variables]] that affect foliage.

- `foliage.ForceLOD <NUMBER>`: Force a specific LOD for all [[Static Mesh Foliage]].
	- The [[Movie Render Queue]] does this automatically, but it can be disabled by disabling Game Overrides > Force LOD 0 in the [[Movie Render Queue]] settings.


# References

- [_UE5 Hints & Tips - Foliage and Trees_, by UnrealityBits @ youtube.com](https://www.youtube.com/watch?v=dxofebT02VI)


