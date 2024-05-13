To import a [[Static Mesh]] click the Import button in the [[Content Browser]] and select an FBX file.
The FBX Import Options dialog opens.
The below is a description of some of the FBX import options, and the selections I usually make for each when importing a [[Static Mesh]].

- Mesh
  - Skeletal Mesh: Off. Only enable this if you want to import a [[Skeletal Mesh]].
  - Auto Generate [[Collision]]: On, unless the source asset has `U[BCS]X_` submeshes. Have the engine generate a basic collision setup for the mesh. I don't know how it decides what collision setup to make, but it is good to get at least something. It can be changed or deleted later in the [[Static Mesh Editor]].
  - Combine Meshes: On. Many mesh DCC tools support building models from pieces. This settings controls whether the pieces should be imported as separate [[Static Mesh]] assets or if they should be merged into a single [[Static Mesh]]. I don't know if this will do any triangle or vertex processing, or if it will do a simple vertex union and vertex index offsetting.
  - Normal Import Method: Import Normals. Not sure why not Import Normals And Tangents. Do most mesh DCC tools not generate tangents? Do Unreal Engine have its own definition of tangents that is different from most DCC tools?
  - Normal Generation Method: Mikk TSpace. This seems to be the standard. Make sure you DCC tool supports Mikk Tangent Space and that it is enabled.
- Miscellaneous
  - Convert Scene: On. Convert to the Unreal Engine [[Coordinate System]] from the FBX coordinate system. This ensures that up / forward / right directions are coorect. Not sure how this correlate with the coordinate settings in the DCC tool.
  - Convert Scene Units: On. Convert to the Unreal Engine [[Units]]. This ensures that the mesh has the correct size. Not sure how it determines what the source units are. Is that stored within the FBX file? Does the FBX format dictate a length unit?
- Material
  - Material Import Method: Do Not Create Material. Most DCC tools do not have the same [[Material]] implementation as Unreal Engine, so better to make the [[Material]] in the [[Material Editor]] rather than importing it. Unless the [[Material]] is very simple, such as a single solid color.


[[Coordinate System]] [[Units]]

The imported mesh may contain [[Collision Shape|Collision Shapes]].
The should be named `UBX_<MESH_NAME>_#` for box collision shapes and `USX_<MESH_NAME>_#` for sphere collision shapes, where `#` is a counter counting the number of collision shapes.

After the mesh has been imported you can modify the source asset, export again, and then click the Reimport Base Mesh button in the [[Static Mesh Editor]] to replace the triangles in the [[Static Mesh Asset]] with the newly exported triangles.
# References

- [_Becoming an Environment Artist in Unreal Engine_ > _Scale Prep for Unreal Usage_ by Epic Games @ dev.epicgames.com/courses 202 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/JGl/unreal-engine-scale-prep-for-unreal-usage)
- [_Becoming an Environment Artist in Unreal Engine_ > _Importing Meshes and Textures Into Unreal_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/aVW/unreal-engine-importing-meshes-and-textures-into-unreal)


