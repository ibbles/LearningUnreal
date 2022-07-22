There is a [[View Mode]] named Optimization Viewmodes > Light Complexity.
For more details see [[Rendering Performance Profiling And Analysis]].

Keep as many lights as possible [[Static Lighting|Static]] and take the computation hit at built time instead of run time.

In the [[Light Source Properties]] and [[Static Mesh]] Details panel:
- Keep all light's Details panel > Light > Attenuation Radii as small as possible. This is the distance that light from the light source can illuminate a surface. It is camera independent. Don't let the circle be larger than the visible contribution of the light.
- Keep all light's Details panel > Performance > Max Draw Distance as small as possible. This is the max distance from the camera that a light source will be considered. If the light source is farther away than this it will be disabled. Also set a Max Draw Distance Fade Range to make the light fade out instead of sharply cut off. Not sure if the range is `[max - fade, max]` or `[max, max + fade]`.
- Set shadow settings appropriately, whatever that means.
	- If you don't need shadows from a particular light then turn them off with Details panel > Light > Cast Shadows.
	- Consider turning shadows off for [[Foliage]] because large shadows are heavy and foliage, acting as one giant mesh, often have very large shadow volumes.
	- Turn off [[Dynamic Shadows|Signed Distance Field Shadows]] on tiny meshes. Signed Distance Field Shadows are for distant objects and a tiny object far away will have shadow that won't be visible anyway. [[Static Mesh Asset]] > Details panel > Lighting > Advanced > Affect Distance Field Lighting > Disable or set Distance Field Resolution Scale to 0.0.
(
TODO: Learn more about shadows, their settings, and how they affect performance.
)

The most computationally heavy part of [[Lighting]] is [[Shadows]].
Turn off Details panel > Light > Cast Shadow on as many lights as possible.
If you must have shadows from a light, consider only having shadow casting enabled when needed, have some kind of game logic to turn it on and off as needed.
Have game logic that detect cases where a light doesn't contribute much, such as when a door is closed, and disable lights inside the room.


# Landscape

[[Landscape]] has a [[Lightmap]] resolution setting at Details panel > Lighting > Static Light Resolution.
This is the per-quad-side resolution of the [[Lightmap]].


# References

- [_5 Tips to Optimize Environments in Unreal Engine 4_ - Lighting, by Jakub Haluszczak @ youtube.com 2021](https://youtu.be/gZkKcaF4Ifk?t=505)

