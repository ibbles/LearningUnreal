The goal of visibility culling is to reduce the number of objects that is to be rendered.
Determine which things can be seen.
Consists of a sequence of steps, from cheap to expensive, that removes objects.

## Distance Culling
Cull Actors that are far away from the camera.
The cull distance is set on any renderable thing.
On a Static Mesh it is set at Rendering > LOD > Desired Max Draw Distance.

There is also Cull Distance Volume that will set a cull distance based on size.
It contains an array of (Size, Cull Distance) pairs.
Any Actor whose size is within the range between two consecutive Size elements will be culled at the first Size's Cull Distance.
Example:
```
Size	Cull Distance
0.0		4'500
512		7'000
1'024	14'000
```
An object of size 200 has a size that is between 0.0 and 512. It will therefore be culled at the shorter distance, 4'500 units.
An object of size 1'000 has a size that is between 512 and 1024. It will therefore be culled at the shorter distance, 7'000 units.
Any object larger than 1'024 will be culled when it is farther than 14'000 units.

You should almost always have Distance Culling set.

## Frustum Culling
Unreal Engine does not render things that are outside of the visible area of the camera.

## Precomputed Visibility
Mostly for mobile and other low-end devices.
Take a higher memory cost for reduced computational cost.
Enabled in Main Tool Bar > Settings > World Settings > Precomputed Visibility > Precompute Visibility.
The Advanced section of Precomputed Visibility has settings such as Visibility Cell Size.

Based on spatial cells.
The world is split into cells and each cell keep a list of objects that are visible from that cell.
It is not necessary to cover the entire world in cells.
Specify where cells should be created with a Precomputed Visibility Volume.
Can be visualized with Viewport > Top Left Buttons > Show > Visualize > Precomputed Visibility Cells.

The visibility computation is done as part of [[Building Lighting|lighting building]].

## Visibility Culling
### Hardware Occlusion
Every mesh acts as an occluder for everything else.
This can be visualized with the `freezerendering` console command.
This will freeze the current set of occluded meshes while allowing the camera to move.
Making it possible to see what isn't rendered at a particular camera location.

Not sure what the "hardware" part in Hardware Occlusion is.
Not supported on all devices, such as some mobile devices.

### Software Occlusion
Not often used, fallback when Hardware Occlusion can't be used.
Pick one of the mesh's LODs to act as an occluder.
Set in General Settings > Advanced > LOD For Occluder Mesh.
I assume this is in the Statich Mesh settings.
