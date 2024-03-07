An Instanced Static Mesh Component is a Scene Component that renders a [[Static Mesh Asset]] multiple times at different locations.
Select a [[Static Mesh Asset]] either from the [[Details Panel]] or by calling the Set Static Mesh member function.
Add an instance by calling the Add Instance member function.
Each instance must have a [[Transform]].
To move the instances use `UInstancedStaticMeshComponent::BatchUpdateInstancesTransforms`.
This function has the `NewInstancesPrevTransforms`, which ensures that the mesh is moved, as opposed to teleported, to the new location, which is required to prevent [[Ghosting Smeary Rendering]].
