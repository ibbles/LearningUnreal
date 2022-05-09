The most computationally heavy part of [[Lighting]] is shadows.
To improve performance, have game logic that detect cases where a light doesn't contribute much, such as when a door is closed, and disable lights inside.

In the [[Light Source Properties]] and [[Static Mesh]] Details panel:
- Keep all light's Attenuation Radii as small as possible.
- Keep all light's Max Draw Distance as small as possible.
- Set shadow settings appropriately, whatever that means.
  - Consider turning shadows off for [[Foliage]] because large shadows are heavy and foliage, acting as one giant mesh, often have very large shadow volumes.
  - Turn off [[Dynamic Shadows|Signed Distance Field Shadows]] on tiny meshes. Signed Distance Field Shadows are for distant objects and a tiny object far away will have shadow that won't be visible anyway. [[Static Mesh Asset]] > Details panel > Lighting > Advanced > Affect Distance Field Lighting > Disable or set Distance Field Resolution Scale to 0.0.
(
TODO: Learn more about shadows, their settings, and how they affect performance.
)

