A [[Material]] intended for use as a [[Post Processing]] [[Material]].
To use the Post Process Material, add it to the [[Post Process Volume]] > [[Details Panel]] > Rendering Features > Post Process Materials array and make sure the weight floating point value in the array element is larger than 0.0.

# Material Setup

A Post Process Material is a regular [[Material]] on which [[Details Panel]] > Material > Material Domain has been set to Post Process.
A Post Process Material only have a single active input pin on the [[Material Output Node]], the Emissive Color.
This is because a Post Process Material is evaluated over the [[GBuffer]] after all regular rendering and shading has already taken place, and most of the [[Material Output Node]] input pins are input to that rendering and shading process.
Emissive Color is pretty much just a color that we put on the screen.

# Input

The input to a Post Process Material is accessed with a Scene Texture node.
The [[Details Panel]] for this node lets you select which part of the [[GBuffer]] should be sampled.
We have buffers such as Scene Color, Scene Depth, Metallic, Roughness, and so on.
We also have Post Process Input \# buffers.
Post Process Input 0 is the rendered image.
Post Process Input 1 is the output of the first Post Process Material.
Post Process Input 2 is the output of the second Post Process Material.
And so on.

# Techniques And Effects

## Stencil Buffer

A stencil buffer is a single-channel integer buffer that the GPU and read and write.
It can, for example, be used to tell a Post Process Material where different types of objects are by having those objects write a type ID to the stencil buffer.
The stencil buffer is closely related to the depth buffer / depth pass because the stencil buffer is only written precisely when the depth buffer is written, which is during the depth pass.

The values written to the stencil buffer will by themselves not affect the rendered image in any way.
It must be combined with something that reads the stencil values and takes some action based on them.
For example a Post Process Material.

The stencil buffer must be enabled in the [[Project Settings]].
Set [[Project Settings]] > Engine > Rendering > Postprocessing > Custom Depth  - Stencil Pass to Enabled With Stencil.

There is a [[View Mode]] that visualizes the contents of the stencil buffer.
[[View Mode]] > Buffer Visualization > Custom Stencil.

By default nothing is rendered to the stencil buffer.
We can tell a particular [[Static Mesh Actor]] (Which other types have this setting?) to render a particular value to the stencil buffer.
This is done at [[Static Mesh Actor]] > [[Details Panel]] > Rendering > Advanced > Render Custom Depth Pass and the sibling setting Custom Depth Stencil Value.
(
There is also Custom Depth Stencil Write Mask that I don't know what it is or what it is used for yet.
)

With these set, when the [[Static Mesh Actor]] is rendered it will write the Custom Depth Stencil Value to the stencil buffer if no object closer to the camera has already written something to those texels.
With [[View Mode]] > Buffer Visualization > Custom Stencil enabled we can see a color for each stencil value, and the value rendered as text on the object.

What follows is a description of how to create a Post Process Material that renders a solid color on top of where something has been written to the stencil buffer.

Create a new [[Material]] and set [[Details Panel]] > Material > Material Domain to Post Process.
Create two Scene Texture nodes.
For one of them, set [[Details Panel]] > Scene Texture ID to Post Process Input 0.
For the other, set [[Details Panel]] > Scene Texture ID to Custom Stencil.
The goal is to either use the Post Process Input 0 color as-is when there is nothing in the stencil buffer, and to use a tinted version of that color when there is something in the stencil buffer.
The tinted color is produced by multiplying the Post Process Input 0 color with some tint color.
Pass the unedited Post Process Input 0 color and the tinted color to a [[Lerp]] node.
Connect the output of the Custom Stencil node to the Alpha of the [[Lerp]] node.

Create a [[Post Process Volume]] in the [[Level]].
Add an element to [[Post Process Volume]] > [[Details Panel]] > Rendering Features > Post Process Materials.
In the Choose drop-down select Asset Reference.
Drag the Post Process Material we just created to the [[Asset]] reference.
If there is an object in the [[Level]] that writes to the stencil buffer then those pixels will get the tint color from the Post Process Material.
I don't know how to make the tint color only appear when the object is behind some other object.

You may notice that the tint has jittery edges.
This is an effect of anti-aliasing.
This can be mitigated by setting [[Material]] > [[Details Panel]] > Post Process Material > Blendable Location to Before Tonemapping.
This may change the look of the Post Process Material.
