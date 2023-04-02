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
See _Using A Runtime Virtual Texture As A Landscape Material Cache_ below for an example.

(
I still don't understand when the Virtual Texture part of the [[Material]] is run.
I don't think it's every frame since Runtime Virtual Textures are supposed to provide a performance gain.
Is it only the first frame that the object is visible?
Will that produce a gigantic render time the first frame when a whole bunch of things want to render to their Runtime Virtual Texture all at the same time?
Does [[LOD]] level switching trigger a new render?
Things going in and out of view?
)

Must be enabled with [[Project Settings]] > Engine > Rendering > Virtual Textures > Enable Virtual Texture Support.

Any rendered object, such as a [[Landscape]] or a [[Static Mesh]], that writes to one or more Runtime Virtual Textures must declare which Runtime Virtual Textures it writes to in Details panel > Virtual Texture > Render To Virtual Textures.
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

UE 5.1: There is now also From Virtual Texture.
By Virtual Texture I don't think they mean the Runtime Virtual Texture asset referenced in the Draw In Virtual Textures array.
It doesn't have any property that it makes sens to affect this.
Instead I think it means the Runtime Virtual Texture Volume [[Actor]] in the level that has that has its Virtual Texture [[Property]] set to the same Runtime Virtual Texture [[Asset]].
The Runtime Virtual Texture Volume [[Actor]] has a Boolean [[Property]] named Hide Primitives.

My theory is that setting the renderer to From Virtual Texture  and the Runtime Virtual Texture Volume's Hide Primitives to `true` should make the renderer only render to the Runtime Virtual Texture and therefore not show up as a regular mesh in the [[Viewport]].
Setting the Boolean to `false` should make the mesh render to both the [[Viewport]] and the Runtime Virtual Texture.
However, in testing with Hide Primitives set to `false` I still don't see the renderer during Play In Editor.
Hide Primitives does hide/unhide the render as expected in regular edit mode.
)


# Quick Usage Summary

- Create one or more Runtime Virtual Texture assets in the Content Browser.
- Set a size and a layout for each Runtime Virtual Texture.
- For each Runtime Virtual Texture, create and place a Runtime Virtual Texture Volume and set Details panel > Virtual Texture to one of the Virtual Texture assets.
- On the object ([[Static Mesh]], [[Skeletal Mesh]], [[Landscape]], [[Foliage Type]]) that should write to the Runtime Virtual Texture add the Virtual Texture asset to Details panel > Virtual Texture > Draw In Virtual Textures.
- In the [[Material]] used by the object that should write to the Runtime Virtual Texture add a Runtime Virtual Texture Output node.
- Connect something to each of the input pins on the Runtime Virtual Texture Output node for which there exists a Runtime Virtual Texture channel in the rendered object's Draw In Virtual Textures array, i.e. a Runtime Virtual Texture that includes that data in its layout.
- In the [[Material]] for the object that should read from the Runtime Virtual Texture add a Runtime Virtual Texture Sample node, or a Runtime Virtual Texture Sample Parameter node.
- Set which Runtime Virtual Texture the sample node should read from, either on the node directly (for the non-parameter node) or on a [[Material Instance]] (for the parameter node).
- Set Runtime Virtual Texture Sample (Parameter) > Details panel > Virtual Texture > Virtual Texture Content to the same layout as on the Runtime Virtual Texture asset.
- Pass the outputs of the Runtime Virtual Texture Sample node to the [[Material Output Node]], possibly with some alterations, such as blending with other material computations.



# Creating A Runtime Virtual Texture

## Asset

A Runtime Virtual Texture is an [[Asset]] that lives in the [[Content Browser]].
Create a new Runtime Virtual Texture by [[Content Browser]] > right-click > Textures > Runtime Virtual Texture.

A Runtime Virtual Texture has a size.
The size is expressed in tiles, and we can also set the size of each tile (In texels, I assume).
The size seems to be expressed in powers-of-two, so setting size to 8 gives 256 tiles and a size of 10 gives 1024.
So is the size the side length, and the number shown the total number of tiles over the entire area?

I'm not sure if we pay a memory cost for the entire size of the texture in some way.
The point of the "virtual" part of the name is that only parts of the texture need to be loaded, but does the other parts need to exists at all in some way?
Maybe just as a set of entries in a page table or something?

A Runtime Virtual Texture can have various types of content, also called its Layout.
> TODO Write path to the setting here.
- Base Color
- Base Color + Normal + Roughness + Specular
- World Height

The list of available layouts varies between Unreal Engine versions.

With a [[Landscape]] it is common to create a Base Color + Normal + Roughness + Specular Runtime Virtual Texture and a World Height Runtime Virtual Texture.
This makes it possible to blend the [[Landscape]] into other rendered object in the other object's [[Material]].


## Volume

A Runtime Virtual Texture exists within a volume of the [[Level]].
The volume defines what is being captured by the Runtime Virtual Texture.
That volume is defined by placing a Runtime Virtual Texture Volume [[Actor]].
Set the Location and Scale of the Runtime Virtual Texture Volume to cover what you need it to.
There is a Details panel > Transform From Bounds > (Copy|Set) Bounds button that sets the Runtime Virtual Texture Volume's Location and Scale to contain the bounding volume of some other [[Actor]].

The Runtime Texture Volume must know which Runtime Virtual Texture it belongs to.
Set with Runtime Virtual Texture Volume > Details panel > Virtual Texture > Virtual Texture, point to the Runtime Virtual Texture asset.

We cannot reuse the same Runtime Virtual Texture Volume for many Runtime Virtual Texture assets, unfortunately.


# Writing To A Runtime Virtual Texture In A Material


## Rendered Object Configuration

A write will only take place if the object begin rendered, such as a [[Static Mesh]], [[Landscape]], or [[Foliage]], has a Runtime Virtual Texture assigned and that Runtime Virtual Texture has a layout that includes the thing being written.
Assign which Runtime Virtual Texture an object should write to with [[World Outliner]] > select the object > Details panel > Virtual Texture > Render To Virtual Textures.
The object must also have Details panel > Virtual Texture > Virtual Texture Pass Type set to something that includes Virtual Texture.
When rendering an object using a [[Material]] the writes to a Runtime Virtual Texture, list those Runtime Virtual Textures in Landscape > Details panel > Virtual Texture > Draw In Virtual Textures.


## Runtime Virtual Texture Output Node

To write to a Runtime Virtual Texture from a [[Material]] add a Runtime Virtual Texture Output node to the [[Material]].
There can only be a single Runtime Virtual Output node in  a [[Material]].
Or rather, there cannot be two writes to the same Runtime Virtual Texture member, but different members can be written in different Runtime Virtual Texture Output nodes.
The Runtime Virtual Texture Output node has input pins for all that that can be stored in a Runtime Virtual Texture.
- Base Color
- Specular
- Roughness
- Normal
- World Height
- Opacity
- Mask
- (More?)

Most of the input pins on the Runtime Virtual Texture Output node can be moved from the [[Material Output Node]].

**World Height** is not computed by most [[Material]]s. Use an Absolute World Position node (called WorldPosition in the node list) and a Component Mask node to extract just the B (Z) component, and connect that to Runtime Virtual Texture Output > World Height.

The **Normal** passed to Runtime Virtual Texture output should be in [[World Space]], while the Normal passed to the [[Material Output Node]] is in [[Tangent Space]].
Convert from [[Tangent Space]] to [[World Space]] with a Transform Vector node, called Vector Ops > Transform in the list.
(
Does the normal have to be in World Space?
What happens if we keep it in Tangent Space?
What happens if we store a normal from a [[Landscape]] in Tangent Space and then use it unchanged also in Tangent Space in the [[Material]] on another object?
The [[Landscape]] triangle probably isn't aligned in the same orientation as the triangle on the other object.
World Space is a sort of mutual communication medium, both the [[Landscape]] and the other object can interpret a normal in World Space.
So by converting Landscape's Tangent Space > World Space > other object's tangent space we get a vector that has the same direction, but in the format the the rendering pipeline needs.
)

If everything is working then the result of the write is visible in the Runtime Virtual Texture preview in the Details panel > Virtual Texture > Draw In Virtual Textures array.


# Reading From A Runtime Virtual Texture In A Material

Reading a Runtime Virtual Texture in a [[Material]] is done with either the Runtime Virtual Texture Sample node or the Runtime Virtual Texture Sample Parameter node.
In the Details panel for the Runtime Virtual Texture Sample node set Virtual Texture to the Virtual Texture [[Asset]] to read from.
Alternatively, if using a Runtime Virtual Texture Sample Parameter node, assign a Runtime Virtual Texture asset in a [[Material Instance]].
In Sample node >  Details panel > Virtual Texture > Virtual Texture Content select the same layout as the Runtime Virtual Texture asset has.
The Sample node has output pins for the regular Runtime Virtual Texture stuff.
You may only read from pins that match the Virtual Texture Content setting of the Sample node and the layout of the Runtime Virtual Texture asset.
Not sure what happens if we read attributes that the Runtime Virtual Texture [[Asset]] doesn't contain, I assume we get zeros.

The **Normal** read from a Runtime Virtual Texture is typically in [[World Space]].
Unless the [[Material]] has Tangent Space Normal disabled in the Details panel we must transform it to Tangent Space.
You can use a Vector Ops > Transform node to do that.
If you intend to blend the normal from the Runtime Virtual Texture with the rendered objects normal, be it from a normal map or the mesh data, you must do this blend using World Space normals.


# Blending A Landscape Material Into Other Objects

This example is based on [_How to Blend Objects with Your Landscape - UE4 Runtime Virtual Texturing (RVT) Tutorial_ by Unreal Sensei @ youtube.com 2021](https://www.youtube.com/watch?v=xYuIDFzKaF4).

The goal is to use a Runtime Virtual Texture to blend a [[Landscape]]'s [[Material]] into another object's [[Material]] for the parts of the object that is very close to the [[Landscape]].
The intention is to make the object feel more grounded, as if sand or dust or other fine particles of the ground surface has migrated up onto the object.

[11:18 - Read Landscape Height To Generate Blend Alpha](https://youtu.be/xYuIDFzKaF4?t=678)
Create a Runtime Virtual Texture Sample node, named , and bind it to the World Height Runtime Virtual Texture.
Set Details panel > Virtual Texture > Virtual Texture Content to World Height.
The World Height read from the Runtime Virtual Texture, i.e. the [[Landscape]]'s height at this point, will be compared with the height of the fragment of the object that is being rendered.
If the fragment is close to the [[Landmass]] then more of the [[Landscape]]'s [[Material]] will be blended in.
To get the world position of the fragment use the Coordinates > World Position node, called Absolute World Position once created.
Use a Component Mask, just Mask in the graph, node to get only the B (Z) component, i.e. the height.
Subtract the [[Landscape]] height from the fragment height to find the distance between the two. i.e. how high above the [[Landscape]] that the fragment is.
This value should be scaled since we don't always want the blend interval / distance to be 1 cm.
It is advisable to include Object Bounds in the calculation, so that larger objects get a larger blend interval.
At least that's what I think the example is doing, I'm not sure. The math makes no sense to me.
The end result is a [0.0, 1.0] value, using a Saturate node, that is fed to the Alpha input pin of a Blend Material Attributes node.

[17:09 - Read Landscape Material To Blend Materials](https://youtu.be/xYuIDFzKaF4?t=1029)
Create a Runtime Virtual Texture Sample Parameter node, named `VT_Mat`.

[19:49 - Convert Normal Coordinate From World Space To Tangent Space In Reader Material](https://youtu.be/xYuIDFzKaF4?t=1189)
The normal written by the [[Landscape Material]] to the Runtime Virtual Texture is in [[World Space]].
The normal passed to the [[Material Output Node]] should be in [[Tangent Space]].
(Though there is a [[Material]] setting to change this to take a [[World Space]] normal instead, see below.)
However, if you are blending two materials then the two normals should both be in [[World Space]].
(
Why? Why can't two [[Tangent Space]] normals be blended?
We are in the same [[Tangent Space]], i.e. we are in a single [[Material]] for a single object shading a single fragment.
Should be compatible.
)
Transform back to [[Tangent Space]] once you have the final blended normal that is passed to the [[Material Output Node]].
Or disable [[Material]] > Details panel > Material > Advanced > Tangent Space Normal.

[24:00 - Avoiding Texture Stretching On Vertical Surfaces](https://youtu.be/xYuIDFzKaF4?t=1440)
Since Runtime Virtual Texturing is a vertical projection
(
Does it have to be that?
Does it depend on the rotation of the Runtime Virtual Texture Volume?
Does it depend on the rotation of the [[Landscape]]?
What if the [[Landscape]] and the Runtime Virtual Texture Volume have different rotations?
)
any near-vertical surface on the object will all sample from the same Runtime Virtual Texture coordinate, leading to bad stretching. We can avoid this by not including as much, or anything, of the [[Landscape]] [[Material]] contribution in the blending when the normal, in [[World Space]], is nearly vertical.

We can get the World Space normal at the currently rendered fragment with the Vertex Normal WS node and by masking to only the B (Z) channel we get the vertical component.
When this is close to 1.0 then use much of the [[Landscape]] [[Material]] in the blend,
when this is close to 0.0 then use very little of the [[Landscape]] [[Material]] in the blend.
To combine this blend level with the height blend amount from [11:18 - Read Landscape Height To Generate Blend Alpha](https://youtu.be/xYuIDFzKaF4?t=678) simply multiply them.
May need a `1-x` node to get the directions the same.


# Using A Runtime Virtual Texture As A Landscape Material Cache

This example is based on the [Virtual Texturing live stream](https://youtu.be/fhoZ2qMAfa4?t=1153) by Epic Games, which in turn is based on the [Landscape Mountain Sample](https://www.unrealengine.com/marketplace/en-US/product/landscape-mountains) that is available on the Unreal Marketplace.

[Live stream @ 22:38 - Project Settings](https://youtu.be/fhoZ2qMAfa4?t=1358)
Enable [[Project Settings]] > Engine > Rendering > Virtual Textures > Enable Virtual Texture Support.
Make a duplicate of the `M_Landscape_Master` [[Material]] and assign to the [[Landscape]].

[Live stream @ 22:54 - Shader Complexity View Mode](https://youtu.be/fhoZ2qMAfa4?t=1374)
A [[Landscape]] is often expensive because it often samples from many textures due to it blending many layers, and often cover a large fraction of the pixels on screen.
You can use the Shader Complexity [[View Mode]] to determine if you [[Landscape]] is expensive to render.

[Live stream @ 23:13 - Asset And Volume](https://youtu.be/fhoZ2qMAfa4?t=1393)
Create a new Runtime Virtual Texture with [[Content Browser]] > right-click > Materials & Textures > Runtime Virtual Texture.
Set Size Of The Virtual Texture to something that gives a reasonable resolution for the scale of your [[Landscape]].
Set Virtual Texture Content to Base Color, Normal, Roughness, Specular.
Create a Runtime Virtual Texture Volume in the [[Level]] and set the Location and Scale to match the [[Landscape]], optionally using the Copy Bounds button.

[Live stream @ 26:49 - Read VS Write Contexts](https://youtu.be/fhoZ2qMAfa4?t=1609)
The [[Landscape Material]] will exists in two contexts, one to fill in the Runtime Virtual Texture data and one to shade pixels.


## Writing To The Runtime Virtual Texture

[Live stream @ 27:29 Runtime Virtual Texture Output Node](https://youtu.be/fhoZ2qMAfa4?t=1649)
To write to a Runtime Virtual Texture create a Runtime Virtual Texture Output node.
This node has input pins for
- Base Color
- Specular
- Roughness
- Normal
- Opacity

Pass the values you regularly would pass to the [[Material Output Node]] to this node instead.


## Reading From The Runtime Virtual Texture

[Live stream @ 29:15 - Runtime Virtual Texture Sample Node](https://youtu.be/fhoZ2qMAfa4?t=1755)
To read from a Runtime Virtual Texture create a Runtime Virtual Texture Sample node.
Has output pins matching the input pins of the Runtime Virtual Texture Output node, except for Opacity.
The Runtime Virtual Texture Sample node must be bound to a Virtual Texture, which is done in the Details panel.
It must know the Virtual Texture Content of the Virtual Texture, which we set to Base Color, Normal, Roughness, Specular.
The output of the Runtime Virtual Texture Sample goes to the inputs of the [[Material Output Node]].

If there are [[Material Output Node]] input pins that can't be read from the Runtime Virtual Texture, such as [[Emissive Material|Emissive Color]], then those pins can be filled in by regular [[Material]] nodes.
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
(
Some Unreal Engine version newer that 4.23 got the World Height Runtime Virtual Texture layout.
With this we can trivially compare the Z component of the fragment's World Position and the World Height to determine elevation over the [[Landscape]].
)

## Material Domain Set To Virtual Texture

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


## Flat Infinite LOD

[Live stream @ 53:27](https://youtu.be/fhoZ2qMAfa4?t=3207)
Render some geometry into a [[Landscape]]'s Runtime Virtual Texture, for example using the [[Foliage]] technique outlined above, and then also render the actual geometry on top of if.
Have a culling distance for the geometry so that it is removed when far away.
That's what I call "infinite LOD", a LOD level with zero triangles remaining.
A flat version of the object will still be visible on the [[Landscape]].
(
Not sure I understand this correctly.
The presenter talked about the geometry's [[Material]] sampling from the Runtime Virtual Texture.
I don't see why it would need to do that.
As a speed thing? No need to run the object's expensive shader when we can just sample the Runtime Virtual Texture?
That doesn't work all that well for many mesh shapes, because of the vertical triangle problem demonstrated in the _Sample The Landscape Runtime Virtual Texture From Other Materials_ section above.
)


# References

- [_Using Virtual Heightfield Mesh For Landscape Displacement In Unreal Engine 5 (Updated)_, by BuildGamesWithJon @ youtube.com](https://www.youtube.com/watch?v=H4jzMsiBkYg)
- [_Virtual Texturing | Live from HQ | Inside Unreal_ - Runtime Virtual Textures, by Unreal Engine @ youtube.com. 2019](https://youtu.be/fhoZ2qMAfa4?t=1153)
- [_How to Blend Objects with Your Landscape - UE4 Runtime Virtual Texturing (RVT) Tutorial_ by Unreal Sensei @ youtube.com 2021](https://www.youtube.com/watch?v=xYuIDFzKaF4)
- [_How to colour and blend Stylized Grass in UE4 (Read Description for updates!)_, by PrismaticaDev @ youtube.com](https://www.youtube.com/watch?v=iaIDnz88Xbo)
- [_Landscape Communication in UE4 (Automatic Snow/Desert Foliage!)_, by PrismaticaDev @ youtube.com](https://www.youtube.com/watch?v=rW4zCzuGZvs)


