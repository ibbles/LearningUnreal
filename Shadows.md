Shadows can be either [[Dynamic Shadows|Dynamic]] or [[Static Shadows|Static]], which is determined by the [[Light Mobility]] of the [[Light]] source that produces the shadow.
And there are multiple types of both dynamic and static shadows.

There are a few different shadowing methods, or algorithms / implementations, such as [[Virtual Shadow Maps]] and [[Cascaded Shadow Maps]].
Chose one in [[Project Settings]] > Engine > Rendering > Shadows > Shadow Map Method.

There are also [[Distance Field Shadows]], which is enabled per [[Light]].
There are also [[Ray Traced Shadows]], which is enabled per [[Light]].
I'm not sure how these shadow methods interact with the Shadow Map Method setting in the [[Project Settings]].

There are [[Soft Shadows]] that don't have hard edges.
This is not a shadowing method but an effect produced by some of the shadowing methods listed above.

[[Lighting And Shadow Performance]]


# Static Mesh

Shadows can be enabled or disabled per [[Static Mesh Asset]] LOD-level in the [[Static Mesh Editor]].
Open your foliage [[Static Mesh Asset]] in the [[Static Mesh Editor]].
In the Details panel there is a LOD Picker category where we can chose to display settings for the LOD levels available in the mesh.
Pick the largest number / farthest LOD from the list.
This enables the Details panel category for that LOD level.
In the Sections group within that category we have a Cast Shadow checkbox, disable that.
This will make that specific LOD not cast any shadow.
Repeat for as many LOD levels up the list as you need.

A bug in Unreal Engine 5.0 makes all instances of that [[Static Mesh Asset]] in the level to be not rendered, reload the level to get them back again.
