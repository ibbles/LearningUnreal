A Directional Light is a type of [[Light Source]].
A Directional Light is a light source that only has a direction.
The position doesn't matter, the entire scene is affected by the Directional Light equally, except for objects in shadow.
The direction is controlled with the light's Rotation.
It simulates light coming for infinitely far away, i.e., parallel light rays.
Often used for sunlight, since the sun is very far away, or moonlight, since the moon is also far away.
[[Outdoor Lighting]].
Often used for exterior environment or indoor environments with sunlight through a window.

Create a new Directional Light by dragging it in from the Place Actors panel, or using the [[Environment Light Mixer]].

The direction of the Directional Light in a level can be changed by pressing Ctrl+L and moving the mouse.
Not sure how it chooses which Directional Light to change the direction of.

By default a directional light with [[Light Mobility]] set to movable will use [[Cascaded Shadow Maps]].
(
Is this true also when using [[Lumen]]?
Is this true also when using [[Virtual Shadow Maps]]?
)

# References

- [_Lighting in Unreal Engine 5 for Beginners_ by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=235)
- [_Lighting Essential Concepts and Effects_, Lighting Basics - by Epic Games @ dev.epicgames.com](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/W0K/lighting-basics)
 - [_Lighting Essential Concepts and Effects - Dynamic Lighting - Outdoor_, by Epic Games @ dev.epicgames.com, 2018](https://dev.epicgames.com/community/learning/courses/Xwp/lighting-essential-concepts-and-effects/V0M/dynamic-lighting-outdoor)
- [_Becoming an Environment Artist in Unreal Engine_ > _Lighting_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE 4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/oE6/unreal-engine-lighting)
