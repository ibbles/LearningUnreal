See also [[Editor Scalability Settings]].

By default the Unreal Editor reduces CPU usage, which reduces performance and framerate, when it loses focus.
If you need full performance even when Unreal Editor does not have focus, is in the background, then disable [[Editor Preferences]] > Use Less CPU When In Background.

# Optimization Viewmodes

The [[Level Viewport]] supports various [[View Mode|View Modes]].
These are different things that can be rendered instead of the regular view of the scene.
Each [[View Mode]] visualizes a particular feature or aspect of the rendering pipeline.
The [[View Mode|View Modes]] listed below are grouped under Optimization Viewmodes in the [[View Mode]] list.

## Quad Overdraw

Shows how many times each pixel is rendered by the GPU.
Blue is good, red is bad, white is really bad.
One way to reduce overdraw is to create [[LOD]] for your [[Static Mesh|Static Meshes]].
With fewer triangles we get fewer triangles per screen area and thus less overdraw.

[_5 Tips to Optimize Environments in Unreal Engine 4_ - Overdraw, by Jakub Haluszczak @ youtube.com 2021](https://youtu.be/gZkKcaF4Ifk?t=74)

## Required Texture Resolution

Shows the ratio between the currently streamed texture resolution and the resolution wanted by the GPU.
To use you must select a [[Static Mesh]] in the [[Level Viewport]] and a texture from the new button that appeared at the top of the [[Level Viewport]] when the Required Texture Resolution [[View Mode]] was enabled.
A color indicate whether or not there is sufficient texture data to render the current view of the [[Static Mesh]].
Move the camera closer to make the color go towards red, move the camera further away to move the color towards green.
You want to have the texture in a size that makes it render in white when at a typical viewing distance.
If the [[Static Mesh]] is highlighted in green even when small / far away then you can safely reduce the size of the texture.
(
How?
)

[_5 Tips to Optimize Environments in Unreal Engine 4_ - Texture Size, by Jakub Haluszczak @ youtube.com 2021](https://youtu.be/gZkKcaF4Ifk?t=461)

## Light Complexity

Color-code each pixel based on how expensive lighting calculation was performed for that pixel. (I think.)
Blue is good, red is bad, white is really bad.
See [[Lighting And Shadow Performance]] for tips on how to improve lighting performance.

Not sure what goes into Light Complexity and what goes into Shader Complexity.

## Shader Complexity

Not sure what goes into Shader Complexity and what goes into Light Complexity.


# Statistics Window

You can get an overview of what the engine is rendering with
- Unreal Engine 4: Top Menu Bar > Window > Statistics.
- Unreal Engine 5: Top Menu Bar > Tools > Audit > Statistics.

Shows a table with the objects in the scene and some of their properties, such as which [[Actor|Actors]] the object is used by, its type, number of triangles, memory footprint, and bounding sphere radius.
You can click a cell in the Actors column to select and focus on that or those Actors.


# Stat

There are a bunch of [[Stat]] collectors that are useful to analyse the performance of your scene.
These are enabled with a console command and are displayed as a table in the [[Level Viewport]].

- `stat streaming`: Statistics about the texture streaming system.
> TODO List a few more here.


## GPU Visualizer ProfileGPU

Unreal Editor contains a single-frame profiler called [[GPU Profiler]] or GPU Visualiser.
It is opened by running `ProfileGPU` in the [[Console]].
The GPU Visualizers displays a timeline of the frame render and a tree view of the task hierarchy.

# Unreal Insights

[[Unreal Insights]]



# References
- [_Unreal* Engine 4 Optimization Tutorial, Part 1 - GPU Visualizer_, by intel @ intel.com. UE4.19](https://www.intel.com/content/www/us/en/developer/articles/training/unreal-engine-4-optimization-tutorial-part-1.html#gpu-visualizer)


