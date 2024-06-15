An Instanced Static Mesh Component is a Scene Component that renders a [[Static Mesh Asset]] multiple times at different locations.
Select a [[Static Mesh Asset]] either from the [[Details Panel]] or by calling the Set Static Mesh member function.
Add an instance by calling the Add Instance member function.
Each instance must have a [[Transform]].
To move the instances with C++ use `UInstancedStaticMeshComponent::BatchUpdateInstancesTransforms`.
This function has the `NewInstancesPrevTransforms`, which ensures that the mesh is moved, as opposed to teleported, to the new location, which is required to prevent [[Ghosting Smeary Rendering]].


# Converting Many Static Mesh Actors To Instanced Static Mesh Components

If you have a bunch of [[Static Mesh Actor]]s in your [[Level]] you can convert them all to a single [[Actor]] containing an Instanced Static Mesh Component for each [[Static Mesh Asset]] used.
Select all the [[Actor]]s, [[Top Menu Bar]] > Actor > _UE Tools_ > Merge Actors > Batch.
Batch the source Actor Components to use instancing as much as possible. Will generate an Actor with Instanced Static Mesh Component(s).
This will hide the selected [[Actor]]s in the [[Outliner]] and create a new [[Actor]] that has a number of Instanced Static Mesh Components.
The created [[Actor]] can be converted to a [[Blueprint Class]] with the graph network button at the top of the [[Details Panel]] to create something that can be duplicated many times in our [[Level]]s.
