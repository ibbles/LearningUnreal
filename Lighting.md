Lighting, the process of adding [[Light]], is what makes it possible to see the objects in our level.
Lighting can be either [[Dynamic Lighting|Dynamic]], which is computed at runtime, or [[Static Lighting|Static]], which is [[Baked Lighting|Baked]] at buildtime.
Lighting is provided by one or more [[Light Source|Light Sources]].

The Global Illumination method used for dynamic lighting is set in Project Settings > Engine > Rendering > Global Illumination > Dynamic Global Illumination Method.
This is where [[Lumen]] is enabled.

[[Global Illumination]].

By default Unreal Engine uses auto exposure to make dark scenes brighter and bright scenes darker.
This is often good during gameplay, but not helpful during level design.
Use a [[Post Process Volume]] to set a fixed expose level.

Sometimes baked lighting remain in the scene despite removing all the light sources.
You can clear the baked lighting data with [[World Settings]] > Lightmass > Advanced > Force No Precomputed Lighting.
Must build lighting once with Top Menu Bar > Build > Build Lighting Only to update everything.


# Calibration Objects Spheres
It is good to have a few known objects in the scene when setting up lighting.
To have a known frame of reference.
The Engine Content, or Starter Pack not sure, contains `SM_ColorCalibrator`.
This is a [[Static Mesh]] with a bunch of spheres in different colors and a cube with differently colored squares.

We can also create a few spheres of our own.
- Black
	- Base color: 0.04
	- Metalness: 0
	- Roughness: 0.3
- White
	- Base color: 0.85
	- Metalness: 0
	- Roughenss: 0.3
- Grey
	- Base color: 0.18
	- Metalness: 0
	- Roughness: 0.3
- Chrome
	- Base color: 0.9
	- Metalness: 1
	- Roughness: 0.05

If we can clearly tell the shape of each of the spheres in a scene then that tells us that the scene is well exposed.
If the scene is under exposed then the black sphere will disappear in the shadows.
Increase Post Process Volume > Details panel > Exposure > Exposure Compensation or Min|Max Brightness.
If the scene is over exposed then detail is lost in the white sphere, it becomes a flat circle instead of a sphere.
Decrease Post Process Volume > Details panel > Exposure > Exposure Compensation or Min|Max Brightness.
This is not an exact science, but we want to be able to see details both in the white and the black.

The chrome ball should feel well integrated into the scene, and should accurately reflect whatever is around it.

# References

- [_Lighting in Unreal Engine 5 for Beginners_ - Creating Light Calibration Spheres, by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=968)
- [_Lighting in Unreal Engine 5 for Beginners_ - Using Light Calibration Spheres, by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=2251)
