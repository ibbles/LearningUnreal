A Scene Proxy is the render thread companion of a [[Primitive Component]].
Data held by the [[Primitive Component]] is copied to the Scene Proxy so that it an be used in the rendering process.
This separation is required because the rendering thread is separate from the game thread.

The C++ class that is the base for all Scene Proxy classes is `FPrimitiveSceneProxy`.
For an example see `FStaticMeshSceneProxy`.
We can create our own Scene Proxies.

The mirroring is typically done in the Scene Proxy's constructor.
This part of the code is run on the game thread. (I think.)

A Scene Proxy often contain some or all of the following:
- Material, `UMaterialInterface`.
- Index Buffer, `FRawStaticIndexBuffer`.
- Vertex Factory.
- Material Relevance, `FMaterialRelevance`.
- Is Visible.





# References
- [_Creating a Custom Mesh Component in UE4 | Part 3: The Mesh Component’s Scene Proxy_ by khammassi ayoub at medium.com](https://medium.com/realities-io/creating-a-custom-mesh-component-in-ue4-part-3-the-mesh-components-scene-proxy-6965a3ea4cc9)
