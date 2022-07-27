Light Propagation Volume was deprecated in Unreal Engine 4.26 and should not be used.
Alternative include
- [[Lightmass]] for [[Static Lighting]].
- [[Screen Space Global Illumination]] for [[Dynamic Lighting]].
- [[Ray Tracing Global Illumination]] for [[Dynamic Lighting]].
- [[Lumen]] for [[Dynamic Lighting]].

A technique to achieve dynamic [[Global Illumination]].
Alternatives include [[Lumen]], another dynamic [[Global Illumination]] implementation, and [[Lightmass]], which is a baked/static technique that produces [[Lightmap|Lightmaps]].

Has less detail than [[Lightmass]], but supports dynamic [[Light Source]] and [[Actor|Actors]].
It also does not need any special lightmap UVs for all the meshes.
Require more time per frame to render.
Not as high quality as [[Lightmass]].

Not sure how it compares to [[Lumen]]. Does [[Lumen]] replace Light Propagation Volume?


# Setup

To enable Light Propagation Volume support we must edit `$UE_ROOT/Engine/Config/ConsoleVariables.ini`
Under `[Startup]` add `r.LightPropagationVolume = 1`.
(
The example was for Unreal Engine 4.18.
I this step still required in Unreal Engine 4.27 or the Unreal Engine 5 line?
The console in Unreal Engine 5.0 doesn't auto-complete `r.LightPropagationVolume`.
Perhaps because this variable can't be changed at runtime.
But I should at least be able to determine if it is currently enabled or not.
)

Select one [[Directional Light]] and set its [[Mobility]] to Movable.
On the same [[Directional Light]] enable Details panel > Light > Dynamic Indirect Lighting.

Create a [[Post Process Volume]] and tweak the settings at Details panel > Rendering Features > Light Propagation Volume to your liking.
Size is the size of the volume around the [[Directional Light]] in which [[Global Illumination]] is calculated.
The smaller this size is, the better quality we get within that volume.
The various Bias settings can be tweaked to reduce light leakage through walls and such.


# References

- [_Introducing Global Illumination_ - Dynamic GI, by Epic Games @ dev.epicgames.com, UE-4.20](https://dev.epicgames.com/community/learning/courses/yon/introducing-global-illumination/MnW/dynamic-gi)
- [_Light Propagation Volumes_, by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/4.27/en-US/BuildingWorlds/LightingAndShadows/LightPropagationVolumes/)
- [_Global Illumination_, by Epic Games @ docs.unrealengine.com, UE-4.27](https://docs.unrealengine.com/4.27/en-US/RenderingAndGraphics/GlobalIllumination/)
