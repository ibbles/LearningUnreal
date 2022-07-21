See also [[Editor Scalability Settings]].


# Optimization Viewmodes

The [[Level Viewport]] supports various View Modes.
These are different things that can be rendered instead of the regular view of the scene.
Each View Mode visualizes a particular feature or aspect of the rendering pipeline.


## Optimization Viewmodes > Quad Overdraw

Shows how many times each pixel is rendered by the GPU.
One way to reduce overdraw is to create [[LOD - Level Of Detail]] for your [[Static Mesh|Static Meshes]].
With fewer triangles we get fewer triangles per screen area and thus less overdraw.

[_5 Tips to Optimize Environments in Unreal Engine 4_ - Overdraw, by Jakub Haluszczak @ youtube.com 2021](https://youtu.be/gZkKcaF4Ifk?t=74)

# Statistics Window

You can get an overview of what the engine is rendering with
- Unreal Engine 4: Top Menu Bar > Window > Statistics.
- Unreal Engiene 5: Top Menu Bar > Tools > Audit > Statistics.

Shows a table with the objects in the scene and some of their properties, such as which [[Actor|Actors]] the object is used by, its type, number of triangles, memory footprint, and bounding sphere radius.
You can click a cell in the Actors column to select and focus on that or those Actors.

