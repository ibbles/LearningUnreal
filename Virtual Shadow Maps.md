Virtual Shadow Maps is a method for generating [[Shadows]].
Virtual Shadow Maps is a project wide setting.
It is enabled by setting [[Project Settings]] > Engine > Rendering > Shadows > Shadow Map Method to Virtual Shadow Maps.
When enabled it replaces [[Cascaded Shadow Maps]].
It is not possible to control this setting from a Post Process Volume. (I think.)
It typically produces higher quality shadows compared to regular [[Shadow Maps]] and [[Cascaded Shadow Maps]].
(
Though I have seen some very blocky / jagged shades when using Virtual Shadow Maps.
Why? How improve quality?
)
In some scenes it comes with a large performance cost.
In particular when there are many moving or animated ([[Skeletal Mesh]] or [[World Position Offset]]) objects.
Enabling [[Nanite]] on each [[Static Mesh]] is supposed to reduce the performance cost of Virtual Shadow Maps.
Does this apply to the light occluder, the shadow receiver, or both?

When Details panel > Distance Field Shadows > [[Distance Field Shadows]] is enabled on a [[Light Source]]
it seems Distance Field Shadows completely replace Virtual Shadow Maps.
This is different from when [[Cascaded Shadow Maps]] is the project default.
In that case the [[Distance Field Shadows]] doesn't kick in until the object is farther away from the camera than the Dynamic Shadow Distance set under [[Cascaded Shadow Maps]] on the [[Light Source]].


# Editor Viewport Visualization Modes

There are a few [[View Mode|View Modes]] related to Virtual Shadow Maps that can be enabled in the Unreal Editor viewport.
They are under Viewport > [[View Mode]] > Virtual Shadow Map.

## Cached Pages

Color areas where shadows are or are not recalculated from frame to frame.
Shadow recalculation is expensive, so we want as much green as possible.
I do not know what the different shades of red means.
For each red area, try to understand what is causing the shadow recalculation and try to find a way to avoid it.
A common thing for recalculation is when something moves.
I assume either a [[Light]] or a [[Skeletal Mesh|Skeletal-]] or [[Static Mesh]]. What else?
See _Foliage_ below for possible workarounds.


# Console Variables

There are a bunch of [[Console Variable|Console Variables]] that control Virtual Shadow Maps.
They are grouped under the `r.Shadow.Virtual` prefix.

- `ShowStats`
- `Clipmap.FirstLevel`
- `Clipmap.LastLevel`
- `MarkCoarsePagesDirectional`
- `UseFarShadowCulling`
- `NonNanite.IncludeInCoarsePages`
- `UseFarShadowCulling`

This does something to improve performance:
```
r.Shadow.Virtual.NonNanite.IncludeInCoarsePages 0
```


# Foliage

Virtual Shadow Maps can have a very high computation cost on foliage-heavy scenes.
There are a few ways to improve performance.

## Invalidated Virtual Shadow Map Cache

Foliage is often animated to simulate wind, which causes the shadow map cache to be invalidated every frame.
You can see this with the Viewport > top left > Lit > Virtual Shadow Maps > Cached Pages view mode.
Foliage is typically non-Nanite, at least in Unreal Engine 5.0 since it doesn't support World Position Offset in [[Material|Materials]], so we can exclude them with by setting the `r.Shadow.Virtual.NonNanite.IncludeInCoarsePages` [[Console Variable]] to 0.
This will improve performance.
I don't know what the trade-off is, what exactly does this enable and what is the effect on the visuals? What does it break?
It still shows up in red in the Virtual Shadow Map > Cached Pages view mode.


## Far Shadow Culling

Another [[Console Variable]] we can disable is `r.Shadow.Virtual.UseFarShadowCulling`.
This makes a difference when there is foliage in the background / distance.
I don't know what it does though, what the trade-offs are.
Culling usually improves performance, why would we get better performance when we disable it?
Makes no sense to me.


## Disable Shadows On Far LODs

Another way to improve performance of distance foliage is to disable shadows all together on far [[LOD|LODs]] of the foliage mesh.
Open your foliage [[Static Mesh Asset]] in the [[Static Mesh Editor]].
In the Details panel there is a LOD Picker category where we can chose to display settings for the LOD levels available in the mesh.
Pick the largest number / farthest LOD from the list.
This enables the Details panel category for that LOD level.
In the Sections group within that category we have a Cast Shadow checkbox, disable that.
This will make that specific LOD not cast any shadow.
Repeat for as many LOD levels up the list as you need.

A bug in Unreal Engine 5.0 makes all instances of that [[Static Mesh Asset]] in the level to be not rendered, reload the level with Top Meny Bar > File > Recent Levels to get them back again.


## Disable Shadows Completely

Instead of disabling shadows in a particular [[LOD]] on the [[Static Mesh Asset]], we can disable shadows completely on the [[Landscape Grass Type]] using the mesh.
At least [[Dynamic Shadows]], not sure how to disable [[Static Shadows]].
Open you [[Landscape Grass Type]] asset and disable Cast Dynamic Shadow.


# References

- [_UE5: Virtual Shadow Map Performance with Foliage_, by UnrealityBites @ youtube.com](https://www.youtube.com/watch?v=AobyMegpUMg)


