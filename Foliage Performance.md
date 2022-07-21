# Per-Foliage Type Cull Distance

By default painted [[Foliage]] will be drawn at any distance.
This might be well and good for large [[Foliage]] such as trees and boulders, but not so good for grass, twigs, pebbles, and other small objects.
We can set per-[[Foliage Type]] cull distance at [[Foliage Mode]] > Foliage panel > select one or more [[Foliage Type]]s > Details > Instance Settings > Cull Distance.
This value is a min/max range where the first value is where the foliage starts to fade out and the second is where it is completely gone.
0 means ignore/infinite.
min=0 max=0 means that the foliage is always rendered.
min=0 max=1'000 means that the start-fade feature is disabled and the foliage will pop in and out at 1'000 units.
min=900 max=1'000 means that foliage closer than 900 units is rendered will full opacity, foliage between 900 and 1'000 units is gradually faded out with distance from the camera, and foliage farther away than 1'000 is not rendered at all.

# Light Map Resolution

We can reduce the [[Foliage]]'s [[Lightmap]] resolution, which may reduce memory usage and improve performance when using [[Static Lighting]].
The [[Lightmap]] resolution is set per [[Foliage Type]] in [[Foliage Mode]] > Foliage panel > select one or  more [[Foliage Type]]s > Details > Instance Settings > Light Map Resolution.

[_Use Fix and optimize foliage in unreal_ - Light Map Resolution, by Batnobie X @ youtube.com. 2021](https://youtu.be/jcZ5V8qFwgE?t=100)
