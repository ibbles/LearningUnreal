Virtual Shadow Maps is a method for generating [[Shadows]].
Virtual Shadow Maps is a project wide setting.
It is enabled by setting [[Project Settings]] > Engine > Rendering > Shadows > Shadow Map Method to Virtual Shadow Maps.
It typically produces higher quality shadows compared to regular [[Shadow Maps]] and [[Cascaded Shadow Maps]].
In some scenes it comes with a large performance cost


# Editor Viewport Visualization Modes

There are a few view modes related to Virtual Shadow Maps that can be enabled in the Unreal Editor viewport.
They are under Viewport > top left > Lit > Virtual Shadow Map.

## Cached Pages

Color areas where shadows are or are not recalculated from frame to frame.
Shadow recalculation is expensive, so we want as much green as possible.
I do not know what the different shades of red means.
For each red area, try to understand what is causing the shadow recalculation and try to find a way to avoid it.
A common thing for recalculation is when something moves.
I assume either a [[Light]] or a [[Skeletal Mesh|Skeletal]] or [[Static Mesh]]. What else?

# Console Variables

There are a bunch of [[CVars]] that control Virtual Shadow Maps.
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
Foliage is typically non-Nanite, at least in Unreal Engine 5.0 since it doesn't support World Position Offset in [[Material|Materials]], so we can exclude them with by setting the `r.Shadow.Virtual.NonNanite.IncludeInCoarsePages` [[CVars|CVar]] to 0.
This will improve performance.
I don't know what the trade-off is, what exactly does this enable and what is the effect on the visuals? What does it break?
It still shows up in red in the Virtual Shadow Map > Cached Pages view mode.

## Far Shadow Culling
Another [[CVars|CVar]] we can disable is `r.Shadow.Virtual.UseFarShadowCulling`.
This makes a difference when there is foliage in the background / distance.
I don't know what it does though, what the trade-off is.
Culling usually improves performance, why would we get better performance when we disable it?
Makes no sense to me.


## Disable Shadows On Far LODs

Another way to improve performance of distance foliage is to disable shadows all together on far [[LOD - Level Of Detail|LODs]] of the foliage mesh.
Open your foliage [[Static Mesh Asset]] in the [[Static Mesh Editor]].
In the Details panel there is a LOD Picker category where we can chose to display settings for the LOD levels available in the mesh.
Pick the largest number / farthest LOD from the list.
This enables the Details panel category for that LOD level.
In the Sections group within that category we have a Cast Shadow checkbox, disable that.
This will make that specific LOD not cast any shadow.
Repeat for as many LOD levels up the list as you need.

A bug in Unreal Engine 5.0 makes all instances of that [[Static Mesh Asset]] in the level to be not rendered, reload the level to get them back again.

## Disable Shadows Completely

Instead of disabling shadows in a particular [[LOD - Level Of Detail|LOD]] on the [[Static Mesh Asset]], we can disable shadows completely on the [[Landscape Grass Type]] using the mesh.
At least [[Dynamic Shadows]], not sure how to disable [[Static Shadows]].
Open you [[Landscape Grass Type]] asset and disable Cast Dynamic Shadow.


# References

- [_UE5: Virtual Shadow Map Performance with Foliage_, by UnrealityBites @ youtube.com](https://www.youtube.com/watch?v=AobyMegpUMg)

