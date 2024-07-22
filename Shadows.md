Shadows can be either [[Dynamic Shadows|Dynamic]] or [[Static Shadows|Static]], which is determined by the [[Light Mobility]] of the [[Light]] source that produces the shadow, the object that is blocking the light, and the object that is receiving the shadow.

There are multiple types of both dynamic and static shadows.
- [[Virtual Shadow Maps]] - Dynamic
- [[Cascaded Shadow Maps]] - Dynamic
- [[Distance Field Shadows]] - Dynamic but does not support shadow caster animation, e.g. [[Skeletal Mesh]] or [[World Position Offset]].
- [[Ray Traced Shadows]] - Dynamic.
- (Some kind of [[Baked Lighting]] shadow? Does it have a name? Are there multiple types here?)

A particular [[Light Source]] can only have one shadow method selected.

The main shadowing method is selected at [[Project Settings]] > Engine > Rendering > Shadows > Shadow Map Method.
Can be either [[Virtual Shadow Maps]] or [[Cascaded Shadow Maps]].
The other types of shadows either take over when the main shadow method goes out of range,
such as when we run out of [[Cascaded Shadow Maps]] and [[Distance Field Shadows]] take over,
or when a particular [[Light Source]] override the shadow method,
such as when enabling [[Ray Traced Shadows]] on a light.

There are also [[Distance Field Shadows]], which is enabled per [[Light]].
There are also [[Ray Traced Shadows]], which is enabled per [[Light]].
I'm not sure how these shadow methods interact with the Shadow Map Method setting in the [[Project Settings]].

There are [[Soft Shadows]] that don't have hard edges.
This is not a shadowing method but an effect produced by some of the shadowing methods listed above.
To get [[Soft Shadows]] the shadow-casting [[Light Source]] must have a non-zero size,
typically by setting a non-zero source radius.

[[Lumen]] is only for indirect lighting and will not affect direct shadowing at all.

See also [[Lighting And Shadow Performance]]


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


# Missing Shadows

A few things to check if one or more shadows are missing.
- The [[Light Source]] must have Details panel > Light > Cast Shadows enabled.
	- And either Cast Dynamic Shadows or Cast Static Shadows enabled depending on if the occluder is Static or Dynamic. (Mobility Static/Stationary vs Movable.
- The [[Light Source]] must have a non-zero Shadow Resolution Scale.


# References

- [_Is this working Distance Field Shadow_ answer by jblackwell @ forums.unrealengine.com 2024](https://forums.unrealengine.com/t/is-this-working-distance-field-shadow/1798515/2)
