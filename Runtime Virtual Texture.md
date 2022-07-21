A Runtime Virtual Texture is a [[Virtual Texture]] that can be written to from a [[Material]].
Uses the same tile-based streaming approach as a [[Virtual Texture]] would.
Only the tiles visible on screen will be written.

It can be considered a huge procedural texture.
In the sense that its content is filled in procedurally during runtime.

It can be considered a huge render target.
In the sense that it is a texture that we can write to it from the GPU.

It can be considered a shading cache.
For an expensive [[Material]] that doesn't change from frame to frame and isn't camera dependent we can have the expensive [[Material]] fill in a Runtime Virtual Texture with cached material data and then have a second [[Material]] that samples that cache and performs only the final shading calculations.
It is really a single [[Material]] [[Asset]], but it is compiled and run in two different contexts.
See Using A Runtime Virtual Texture As A Landscape Material Cache below for an example.

Must be enabled with [[Project Settings]] > Engine > Rendering > Virtual Textures > Enable Virtual Texture Support.

Any rendered object, such as a [[Landscape]], that writes to one or more Runtime Virtual Textures must declare which Runtime Virtual Textures it writes to in Details panel > Virtual Texture > Render To Virtual Textures.
The rendered object must also specify which passes it should be active in.
The available passes are
- Virtual Texture
- Main Pass

I believe Virtual Texture is the context where the Runtime Virtual Texture is written.
I believe Main Pass is where the [[Material Output Node]] is written.
I believe Virtual Texture OR Main Pass means that if Runtime Virtual Textures is enabled (in the [[Project Settings]], or is there a more granular way to control this?) then the object will be rendered to the Runtime Virtual Texture only, not to the frame buffers, and if Runtime Virtual Textures is not enabled then the object will render normally, without any Runtime Virtual Texture at all.
If Runtime Virtual Textures is enabled then something else must render based on the Runtime Virtual Texture for the object to be visible.
(
Though I'm not sure, and there are some AND / OR going on here as well which I don't understand, so this part need a bit more studying.
)


# Creating A Runtime Virtual Texture

A Runtime Virtual Texture is an [[Asset]] that lives in the [[Content Browser]].
Create a new Runtime Virtual Texture by [[Content Browser]] > right-click > Textures > Runtime Virtual Texture.

A Runtime Virtual Texture has a size.
I'm not sure if we pay a memory cost for the entire size of the texture in some way.
The point of the "virtual" part of the name is that only parts of the texture need to be loaded, but does the other parts need to exists at all in some way?
Maybe just as a set of entries in a page table or something?

A Runtime Virtual Texture can have various types of content, also called its Layout.
- Base Color
- Base Color, Normal, Roughness
- Base Color, Normal , Roughness, Specular
- World Height

A Runtime Virtual Texture exists within a volume of the [[Level]].
That volume is defined by placing and Runtime Virtual Texture Volume [[Actor]].
Set the Location and Scale of the Runtime Virtual Texture Volume to cover what you need it to.
There is a Details panel > Transform From Bounds > Copy Bounds button that sets the Runtime Virtual Texture Volume's Location and Scale to contain the bounding volume of some other [[Actor]].
The Runtime Texture Volume must know which Runtime Virtual Texture it belongs to.
Set with Details panel > Virtual Texture > Virtual Texture, point to the Runtime Virtual Texture asset.


# Writing To A Runtime Virtual Texture In A Material

To write to a Runtime Virtual Texture from a [[Material]] add a Runtime Virtual Texture Output node to the [[Material]].
The Runtime Virtual Texture Output node has input pins for all that that can be stored in a Runtime Virtual Texture.
- Base Color
- Roughness
- Normal
- World Height
- (More?)

When rendering a [[Landscape]] using a [[Material]] the writes to a Runtime Virtual Texture, list those Runtime Virtual Textures in Landscape > Details panel > Virtual Texture > Draw In virtual Textures.

(
Are Landscapes the only thing that can write to a Runtime Virtual Texture?
)


# Reading From A Runtime Virtual Texture In A Material

Reading a Runtime Virtual Texture in a [[Material]] is done with the Runtime Virtual Texture Sample node.
In the Details panel for the Sample node set Virtual Texture to the Virtual Texture [[Asset]] to read from.
The Sample node has output pins for the regular Runtime Virtual Texture stuff.
Not sure what happens if we read attributes that the Runtime Virtual Texture [[Asset]] doesn't contain.


# Using A Runtime Virtual Texture As A Landscape Material Cache

This example is based on the [Virtual Texturing live stream](https://youtu.be/fhoZ2qMAfa4?t=1153) by Epic Games, which in turn is based on the [Landscape Mountain Sample](https://www.unrealengine.com/marketplace/en-US/product/landscape-mountains) that is available on the Unreal Marketplace.

[Live stream @ 22:38](https://youtu.be/fhoZ2qMAfa4?t=1358)
Enable [[Project Settings]] > Engine > Rendering > Virtual Textures > Enable Virtual Texture Support.
Make a duplicate of the `M_Landscape_Master` [[Material]] and assign to the [[Landscape]].

[Live stream @ 22:54](https://youtu.be/fhoZ2qMAfa4?t=1374)
A [[Landscape]] is often expensive because it often samples from many textures due to it blending many layers, and often cover a large fraction of the pixels on screen.
You can use the Shader Complexity [[View Mode]] to determine if you [[Landscape]] is expensive to render.

[Live stream @ 23:13](https://youtu.be/fhoZ2qMAfa4?t=1393)
Create a new Runtime Virtual Texture with [[Content Browser]] > right-click > Materials & Textures > Runtime Virtual Texture.
Set Size Of The Virtual Texture to something that gives a reasonable resolution for the scale of your [[Landscape]].
Set Virtual Texture Content to Base Color, Normal, Roughness, Specular.
Create a Runtime Virtual Texture Volume in the [[Level]] and set the Location and Scale to match the [[Landscape]], optionally using the Copy Bounds button.

[Live stream @ 26:49](https://youtu.be/fhoZ2qMAfa4?t=1609)
The [[Landscape Material]] will exists in two contexts, one to fill in the Runtime Virtual Texture data and one to shade pixels.


## Writing To The Runtime Virtual Texture

[Live stream @ 27:29](https://youtu.be/fhoZ2qMAfa4?t=1649)
To write to a Runtime Virtual Texture create a Runtime Virtual Texture Output node.
This node has input pins for
- Base Color
- Specular
- Roughness
- Normal
- Opacity

Pass the values you regularly would pass to the [[Material Output Node]] to this node instead.


## Reading From The Runtime Virtual Texture

[Live stream @ 29:15](https://youtu.be/fhoZ2qMAfa4?t=1755)
To read from a Runtime Virtual Texture create a Runtime Virtual Texture Sample node.
Has output pins matching the input pins of the Runtime Virtual Texture Output node, except for Opacity.
The Runtime Virtual Texture Sample node must be bound to a Virtual Texture, which is done in the Details panel.
It must know the Virtual Texture Content of the Virtual Texture, which we set to Base Color, Normal, Roughness, Specular.
The output of the Runtime Virtual Texture Sample goes to the inputs of the [[Material Output Node]].

If there are [[Material Output Node]] input pins that can't be read from the Runtime Virtual Texture, such as [[Emissive Materials|Emissive Color]], then those pins can be filled in by regular [[Material]] nodes.
It is OK to mix Runtime Virtual Texture sampling and regular shader code in the shading context of the [[Material]].


## Configuring The Rendered Object

[Live stream @ 30:30](https://youtu.be/fhoZ2qMAfa4?t=1840)
There are a few settings that must be configured on the rendered object, the [[Landscape]] in this case.
At this point the [[Landscape]] will render in all-black.
This is because the [[Material]] doesn't yet know which Runtime Virtual Texture the Runtime Virtual Texture Output node should write to.
In Details panel > Virtual Texture we must add our Runtime Virtual Texture [[Asset]] to the Render To Virtual Textures array, and we must set Virtual Texture Pass Type to Virtual Texture AND Main Pass.
This will make the [[Landscape]] appear again, but with incorrect texture tiling.


## Fixing Texture Tiling

[Live stream @ 32:07](https://youtu.be/fhoZ2qMAfa4?t=1927)
The incorrect tiling is because the [[Material]] uses a Far Blend Distance parameter to control the quality of the [[Landscape]] based on distance from the [[Camera]].
This is a problem because when we write to the Runtime Virtual Texture the concept of a [[Camera]] doesn't exist, it is camera independent.
An approximation of the camera distance is to use the mip-level currently being written.
The [[Landscape Material]] used in the sample project calls a [[Material Function]] named `MF_RB_Land_TexCoords_01`.
This function uses the Camera Position node, which we can't in the Runtime Virtual Texture write context.

[Live stream @ 34:05](https://www.youtube.com/watch?v=fhoZ2qMAfa4)
Instead of the Camera Position node we can use the View Property node.
This node has a View Property setting in the Details panel, which can be set to Virtual Texture Output Level.
This outputs a number that increases with the mip level, i.e. small numbers mean close to the camera, large numbers mean far from the camera.
The distance-to-mip mapping is logarithmic, so we should use a Power node when converting back to a distance, two-to-the-power of the mip level.
Can multiply with a constant as well, the live stream uses 1000.
I think this is all a bit hand-wavy and approximate. Do whatever works for you.

[Live stream @ 37:46](https://youtu.be/fhoZ2qMAfa4?t=2266)
The `MF_RB_Land_TexCoords_01` may be used in more than one of the Runtime Virtual Texture contexts.
The Runtime Virtual Texture Replace node takes two inputs, Default and Virtual Texture Output.
It is a switch node, where one of the inputs is passed to the output when in a Runtime Virtual Texture write context, and the other, Default, is used in all other contexts.
The the Camera Position based distance calculation should be passed to the Default input pin and the View Property node based calculation should be passed to the Virtual Texture Output input pin.


## Fixing Interpolated Material Data

[Live stream @ 38:38](https://youtu.be/fhoZ2qMAfa4?t=2328)
When rendering a [[Landscape]] into a Runtime Virtual Texture each [[Landscape Component]] is rendered as a single quad, and not using the tessellated version.
If the [[Material]] is using interpolated data, such as height or normal, then the single-quad mode won't give enough quality interpolation.
To fix, the [[Landscape]] must be allowed to use higher LOD levels when rendering the Runtime Virtual Texture.
This is set in [[Landscape]] > Details panel > Virtual Texture > Virtual Texture Num LODs, which is 0 by default.
Increasing this a bit, to 6, say, will fix the data interpolation issue.
(
I don't understand what is going on here.
Will increasing this number make it not just render a single quad per [[Landscape Component]]?
LOD 0 is usually the highest-quality LOD, why would allowing more coarse / less detailed LOD levels improve precision?
What does LOD levels even mean for a [[Landscape]]?
What is the meaning of 6 vs 3 vs 24? What is the maximum?
)


## Sample The Landscape Runtime Virtual Texture From Other Materials

[Live stream @ 41:46](https://youtu.be/fhoZ2qMAfa4?t=2506)
We can add a Runtime Virtual Texture Sample node to a [[Material]] used to render other objects, such as a [[Static Mesh]], as well, not just the [[Landscape]].
The sample node will handle texture coordinates properly, meaning that it will read the Runtime Virtual Texture data that is at the same world XY coordinate as the corresponding point on the [[Landscape]].
It's sampling in a top-down projection.
This means that vertical faces on the [[Static Mesh]] will have severe material stretching.

In Runtime Virtual Texture Sample node > Details panel > Virtual Texture select the Runtime Virtual Texture [[Asset]] that the [[Landscape Material]] writes to.
Feed the output of the sample node to a Make Material Attributes node.
Use a Blend Material Attributes node to blend between the regular [[Static Mesh]] [[Material]] attributes and the sampled [[Material]] attributes.
There are many ways to compute the Alpha for the blend node.
One way is to use the Z component of Vertex Normal WS, which is the world space normal at the fragment.
This will produce a blend that uses the Landscape material on top of the [[Static Mesh]], and the mesh's own material on its sides.
I assume another way would be to somehow find the height of the [[Landscape]] compared to the fragment and blend in the [[Landscape]] material only close to its surface.
Don't know how to determine the distance from the [[Landscape]] though.

# Material Domain Set To Virtual Texture

[Live stream @ 45:23](https://youtu.be/fhoZ2qMAfa4?t=2723)
Set [[Material Editor]] > Details panel > Material > Material Domain to Virtual Texture.
This makes the [[Material]] able to write to the Runtime Virtual Texture.
Can be used for [[Landscape]] splines, such as roads.

In the [[Landscape Mode]] select a spline and click Details panel > Select All Connected Segments.
You should turn off Details panel > Landscape Spline Meshes > Cast Shadow when rendering a [[Landscape]] spline to a Runtime Virtual Texture.
(
Why? Should the same be done for other types of objects being rendered to a Runtime Virtual Texture as well?
They didn't for the [[Landscape]].
What if we want the shadow when doing the regular rendering? I.e. we want the object to have a shadow.
)

In Spline > Details panel > Virtual Texture add the [[Landscape]]'s Runtime Virtual Texture asset to Render To Virtual Textures.
The demo has Virtual Texture Pass Type set to Virtual Texture OR Main Pass.
This means that if Virtual Textures are enabled in the [[Project Settings]] then the spline will only be rendered to the Runtime Virtual Texture, it will not produce any fragments.
There must be something else that samples the Runtime Virtual Texture to produce fragments that are then turned into pixels that we can see on screen.
In the demo that "something" is the [[Landscape]]'s Main Pass, where it samples the Runtime Virtual Texture to fill in the [[Material Output Node]].

The road [[Material]] used to have a mask texture to define the outer bounds of the pavement.
The [[Material]]'s Details panel > Material > Blend Mode was set to Masked.
When we render the spline to a Runtime Virtual Texture we are allowed to set the Blend Mode to Translucent.
(
Were we not allowed to change the Blend Mode before?
Why not?
)
When we switch the Blend Mode from Masked to Translucent the Opacity Mask input pin on the [[Material Output Node]] is disabled and the Opacity input pin is enabled instead.
This will produce a much better transition between the spline and the surrounding [[Landscape]].


## Foliage Writing To A Runtime Virtual Texture

[Live stream @ 48:59](https://youtu.be/fhoZ2qMAfa4?t=2939)
The screen sharing froze a bit here, but I think the foliage mesh's [[Material]] was opened and Details panel > Material > Material Domain was set to Virtual Texture.
In the [[Foliage Mode]] select the Paint tab, select one or more [[Foliage Type]] and in Details > Virtual Texture set Render To Virtual Texture to the [[Landscape]]'s Runtime Virtual Texture [[Asset]].
Set Virtual Texture Pass Type to Virtual Texture OR Main Pass.
Disable Instance Settings > Cast Shadow.
Set Details > ??? > Translucency Sort Priority to 3.
Translucency Sort Priority control the order in which things are blended into the Runtime Virtual Texture.
The [[Landscape]] is -1 (Is it always the [[Landscape]] that is -1 or can e.g. a [[Static Mesh]] be the base object as well?).
In the example the spline was 0 which means that it is blended on top of the [[Landscape]].
By setting the Translucency Sort Priority of the [[Foliage]] to something larger than that for the spline the [[Foliage]] meshes are rendered on top of the spline.

The effect of rendering [[Foliage]] like this is that they don't create actual geometry in the final image.
We render actual geometry in the Virtual Texture pass into the Runtime Virtual Texture, but from that point on they are just a texture.
This means that the [[Foliage]] appear as a flat surface on the [[Landscape]], much like a decal.


# References

- [_Using Virtual Heightfield Mesh For Landscape Displacement In Unreal Engine 5 (Updated)_, by BuildGamesWithJon @ youtube.com](https://www.youtube.com/watch?v=H4jzMsiBkYg)
- [_Virtual Texturing | Live from HQ | Inside Unreal_ - Runtime Virtual Textures, by Unreal Engine @ youtube.com. 2019](https://youtu.be/fhoZ2qMAfa4?t=1153)
