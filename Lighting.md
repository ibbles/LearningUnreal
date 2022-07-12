Lighting, the process of adding [[Light]], is what makes it possible to see the objects in our level.
Lighting can be either [[Dynamic Lighting|Dynamic]], which is computed at runtime, or [[Static Lighting|Static]], which is [[Baked Lighting|Baked]] at buildtime.
Lighting is provided by one or more [[Light Sources]].

The Global Illumination method used for dynamic lighting is set in Project Settings > Engine > Rendering > Global Illumination > Dynamic Global Illumination Method.
This is where [[Lumen]] is enabled.

[[Global Illumination And Indirect Lighting]].

By default Unreal Engine uses auto exposure to make dark scenes brighter and bright scenes darker.
This is often good during gameplay, but not helpful during level design.
Use a [[Post Process Volume]] to set a fixed expose level.

# Calibration Objects Spheres
It is good to have a few known objects in the scene when setting up lighting.
To have a known frame of reference.
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

# References

- [_Lighting in Unreal Engine 5 for Beginners_ Light Calibration Spheres by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=968)

