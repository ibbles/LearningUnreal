One of the biggest pass in rendering.
It renders all the pieces of geometry in the scene.
Works on all things that a 3D and is to be rendered.
They become a set of draw calls.
The base pass renders a subset of the [[Material]] graph, called Basepass Materials, which includes lightmaps.
It cannot render a complete material because Unreal Engine uses a deferred renderer.

RenderDoc is an application with an Unreal Engine plugin that can be used to visualize the draw calls in a frame.
There is also the Unreal GPU Visualizer that lists the things the renderer does.
It lists the objects that are rendered and how long each one takes, in ms.
(
How does one open the GPU Visualizer?
)

Continue at 13:02