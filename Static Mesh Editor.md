The Static Mesh Editor is where we can configure settings for our [[Static Mesh Asset]].
It is not an editor for the triangles itself, the topology of the mesh.
It is an editor for how Unreal Engine uses and renders the mesh.


# LOD Settings

Where the settings for [[Mesh LOD]] is set.


# Shadows

Shadows can be enabled or disabled per [[Static Mesh Asset]] LOD-level in the [[Static Mesh Editor]].
Open your foliage [[Static Mesh Asset]] in the [[Static Mesh Editor]].
In the Details panel there is a LOD Picker category where we can chose to display settings for the LOD levels available in the mesh.
Pick the largest number / farthest LOD from the list.
This enables the Details panel category for that LOD level.
In the Sections group within that category we have a Cast Shadow checkbox, disable that.
This will make that specific LOD not cast any shadow.
Repeat for as many LOD levels up the list as you need.

A bug in Unreal Engine 5.0 makes all instances of that [[Static Mesh Asset]] in the level to be not rendered, reload the level to get them back again.
