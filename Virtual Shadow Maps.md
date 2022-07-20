
This does something to improve performance:
```
r.Shadow.Virtual.NonNanite.IncludeInCoarsePages 0
```

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

