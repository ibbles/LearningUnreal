See also [[Editor Scalability Settings]].

By default the Unreal Editor reduces CPU usage, which reduces performance and framerate, when it loses focus.
If you need full performance even when Unreal Editor does not have focus, is in the background, then disable [[Editor Preferences]] > Use Less CPU When In Background.


# Optimization Viewmodes

The [[Level Viewport]] supports various [[View Mode|View Modes]] that can be selected from the top-left corner.
The default [[View Mode]] is Lit, which renders the scene using the regular renderer as it would appear in game.
There are different things that can be rendered instead of the regular view of the scene.
Each [[View Mode]] visualizes a particular feature or aspect of the rendering pipeline.
The [[View Mode|View Modes]] listed below are grouped under Optimization Viewmodes in the [[View Mode]] list.

## Shader Complexity

Colors every surface based on the complexity, a proxy for frametime cost, of the [[Shader]] resulting from the [[Material]] used.
Green means good, red means bad but OK in small patches, white means very bad.
A bar at the bottom of the screen shows how complex the [[Vertex Shader]] and [[Pixel Shader]] are individually for the pixel that the cross-hair at the center of the screen is on.
You can see the number of instructions for a material in the [[Stats Panel]] in the [[Material Editor]].
I don't think this count includes extra shader code added by the engine for specific usage scenarios, see [[Shader]].

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


# Statistics Window

You can get an overview of what the engine is rendering with
- Unreal Engine 4: Top Menu Bar > Window > Statistics.
- Unreal Engine 5: Top Menu Bar > Tools > Audit > Statistics.

Can show a number of different tables, selected in the top-left drop-down button.

## Primitive Stats

Shows a table with the objects in the scene and some of their properties, such as which [[Actor|Actors]] the object is used by, its type, number of triangles, memory footprint, and bounding sphere radius.
You can click a cell in the Actors column to select and focus on that or those Actors.

## Texture Stats

Shows a table with all the textures that are currently in memory in the editor.
Not the same as in-game.
The editor often load more than needed.

There is an Actor(s) column that can be used to see which [[Actor]] is using a particular texture.
Can sort by
- Resolution, called Max Dimension in the table, to see what the largest textures are.
- Format, the compression used by the texture.
- Current memory.
- Fully loaded memory.

Use this table to find things that stands out and can be reduced or should be increased.

## Cooker Stats

Shows  from the most recent cook when you last packaged the project and shows the the packaged things in table much like for Texture Stats.
Must have done a packaging for this to work.
Select a platform to show for in the top-right of the window.

# Stat

There are a bunch of [[Stat]] collectors that are useful to analyse the performance of your scene.
These are enabled with a console command and are displayed as a table in the [[Level Viewport]].

- `stat none`: Clear the screen of all statistics.
- `stat fps`: Show an FPS and frametime counter to the right of the [[Level Viewport]].
	- This is the same as enabling [[Level Viewport]] > top-left corner drop-down > Show FPS.
- `stat scenerendering`: A summary of the scene rendering, both timing and contents.
- `stat game`: Blueprint time and such.
- `stat gpu`: 
- `stat unit`: Milliseconds per thread. Useful for knowing where to start looking for bottlenecks.
- `state memory`: Shows memory usage, e.g. how much is used by textures.
- `stat streaming`: Statistics about the texture streaming system.
> TODO List a few more here.


## GPU Visualizer ProfileGPU

Unreal Editor contains a single-frame profiler called [[GPU Profiler]] or GPU Visualiser.
It is opened by running `ProfileGPU` in the [[Console]].
The GPU Visualizers displays a timeline of the frame render and a tree view of the task hierarchy.

# Unreal Insights

[[Unreal Insights]]



# References

- [_Materials Master Learning_ > _Compression and Memory_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/Y0q/compression-and-memory)
- [_Unreal* Engine 4 Optimization Tutorial, Part 1 - GPU Visualizer_, by intel @ intel.com. UE4.19](https://www.intel.com/content/www/us/en/developer/articles/training/unreal-engine-4-optimization-tutorial-part-1.html#gpu-visualizer)
- [_Becoming an Environment Artist in Unreal Engine_ > _Basic Material Creation and Application_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/Ya6/unreal-engine-basic-material-creation-and-application)




