To import a [[Static Mesh]] click the Import button in the [[Content Browser]] and select an FBX file.
The FBX Import Options dialog opens.

- Mesh
  - Combine Meshes: On. Many mesh DCC tools support building models from pieces. This settings controls whether the pieces should be imported as separate Static Mesh assets or if they should be merged into a single Static Mesh. I don't know if this will do any triangle or vertex processing, or if it will be a simple vertex union and vertex index offsetting.
- Miscellaneous
  - Convert Scene: On. Convert to the Unreal Engine [[Coordinate System]] from the FBX coordinate system. Not sure how this correlate with the coordinate settings in the DCC tool.
  - Convert Scene Units: On. Convert to the Unreal Engine [[Units]]. Not sure how it determines what the source units are. Is that stored within the FBX file? Does the FBX format dictate a length unit?


[[Coordinate System]] [[Units]]

# References

- [_Becoming an Environment Artist in Unreal Engine_ > _Scale Prep for Unreal Usage_ by Epic Games @ dev.epicgames.com/courses 202 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/JGl/unreal-engine-scale-prep-for-unreal-usage)


