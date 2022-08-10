A Skeletal Mesh is a triangle mesh that is draped over a [[Skeleton Asset]], like a skin.
Many Skeletal Meshes can use the same skeleton.
A skeleton is a hierarchy of bones.
The shape of the triangle mesh is influenced by the position of these bones.

The Skeletal Mesh contains [[Material Slot|Material Slots]], [[LOD]] settings, post-process animation Blueprints, and more.


# Importing
Skeletal Meshes are typically constructed in a third-party software outside of Unreal Editor, such as Blender, and imported into Unreal Engine.
A common format to import is FBX.
When importing an FBX you can specify that the FBX contains a Skeletal Mesh and that a [[Skeleton Asset ]] should be created in addition to the Skeletal Mesh Asset.

