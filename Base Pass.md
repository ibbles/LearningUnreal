One of the biggest pass in rendering.
It renders all the pieces of geometry in the scene.
Works on all things that a 3D and is to be rendered.
They become a set of draw calls.
Dynamic Instancing means that multiple instances of the same mesh using the same material instance is rendered with a single draw call.
The base pass renders a subset of the [[Material]] graph, called Basepass Materials, which includes [[Lightmap|lightmaps]].
It cannot render a complete material because Unreal Engine uses a deferred renderer.
The output of the Base Pass is the [[GBuffer]].

RenderDoc is an application with an Unreal Engine plugin that can be used to visualize the draw calls in a frame.

There is also the Unreal GPU Visualizer that lists the things the renderer does.
It lists the objects that are rendered and how long each one takes, in ms.
The GPU Visualizer is opened by running the `ProfileGPU` in the [[Console]].

