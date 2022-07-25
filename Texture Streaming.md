The purpose of texture streaming is to maintain high visual quality while managing the available texture memory.
Texture streaming is the act of dynamically loading and unloading [[Texture|Textures]] during runtime.


# Mip-Map Chain

A [[Texture]] contains multiple [[LOD - Level Of Detail|LODs]], different versions of the same texture at different quality levels.
These are called mip levels, or a mip-map chain.
Each level of the chain is half the size of the previous level, with the top level, level 0, being the source texture.
The mip-map chain is generated when the [[Texture]] is imported.
Using a smaller mip level improves performance and reduces aliasing and shimmering if the on-screen texture is smaller than the source texture, i.e. when the texel-to-pixel ratio is large, i.e. when there is a large population of texels that map to the same screen pixel.

Texture streaming ensure that we have the appropriate mip levels loaded in memory, instead of naively loading the entire texture.

Mip-map generation require that the source texture is a power-of-two size in each dimension.
Textures that do not have any mip-maps will not be streamed.

Texture streaming is handled automatically by Unreal Engine.


# Comparison With Virtual Textures

Texture streaming is an alternative to [[Virtual Texture|Virtual Textures]].
The difference between texture streaming and Virtual Texturing is that texture streaming stream different mip levels of the entire texture while Virtual Texturing split the texture into tiles and different quality levels of each tile can be loaded independently of what quality level is loaded for other tiles within the same texture.
A scene can contain a mix of streaming textures and [[Virtual Texture|Virtual Textures]], these are managed independently.



# Texture Streaming Pool

There is a set of video memory that holds these textures called the texture streaming pool.
Unreal Engine uses the texel size and the world bounds (Of each rendered object?) to determine an appropriate texel to pixel ratio for each displayed texture.
(
What is that target ratio?
Can it be configured?
Can it be configured per texture?
)
The camera orientation and view frustum does not appear the influence the choice of mip levels to load, i.e. Unreal Engine considers textures behind the player to be as important as those in front.
I assume this is because a player can turn faster than they can move.

The texture streaming pool has a maximum size.
We can **set this size** with the `r.Streaming.PoolSize` [[Console Variable]].
Setting an appropriate value for your project and target hardware is probably complicated.
I assume this value should be different for different quality settings set by the end user.
You can get some hints to what you need to set it to using the _Statistics_ and _Optimization View Modes_ described below.

If the texture streaming pool is full then a texture mip level that should be loaded may be rejected and we may get a blurry object.
The system may also decide to unload a previously loaded mip level, falling back to a lower mip level for that texture, to make room for a larger mip level of another texture.
The system will prioritize unloading mip levels where the smaller mip level still fulfills the texel to pixel target.

If it is not possible to fit all needed mip levels in the streaming pool then the following message is printed to the top-left corner of the [[Level Viewport]]:
```
TEXTURE STREAMIN GPOOL OVER <AMOUNT> BUDGET
```

The size of the texture streaming pool can be set with the `r.Streaming.PoolSize` [[Console Variable]].
If you set this higher than what the video cards VRAM can hold then you application may crash with an out-of-memory error.

# Reducing Texture Memory Requirement

To reduce the texture memory requirement either:
- Reduce the number of textures that need to be displayed at once, for example by removing all objects that use a particular texture from the view, or by sharing textures between similar objects more.
- Reduce the required mip level for some of the textures, for example by moving objects that use a particular texture further away.
- Reduce the maximum size of a texture, either by actually resizing it in a texture editing software or by specifying a maximum size or mip level for that texture.
	- [[TODO]] I don't know how to set a maximum mip level for a texture.
	- The maximum size of a [[Texture]] is set with [[Texture Editor]] > Details panel > Compression > Advanced > Maximum Texture Size.
		- You can use the [[Property Matrix]] to set Maximum Texture Size for many textures at the same time.
- Set the LOD Bias of the texture in the [[Texture Editor]] to some number of mip levels.
	- This will cause the texture streaming system to load a smaller mip level that it would normally, by the specified number of steps.
	- I believe there is a Device Profile window somewhere where we can set LOD Bias for the entire project.


# Statistics

## Stat

There is a [[Stat]] that displays various statistics about the texture streaming pool.

- Cycle counters
- Memory counters.
	- `Streaming Pool`: The size of the memory pool, i.e. the maximum amount of memory that streaming textures can occupy.
	- `Required Pool`: The amount of memory that the texture streaming system needs to load all the mip levels required to reach the texel to pixel ratio for all considered objects. We want to keep this number below the `Streaming Pool` number at all times. We get the `TEXTURE STREAMING POOL OVER <AMOUNT> BUDGET` message when this value is larger than `Streaming Pool`. The % number on this line is how much of the `Streaming Pool` we want to fill. We want to keep this number below 100%.


## Statistics Panel

There is a Statistics panel that can be opened with
- Unreal Engine 4: Top Menu Bar > Windows > Statistics.
- Unreal Engine 5: Top Menu Bar > Tool > Audit > Statistics.

In the top-left drop-down select Texture Stats.
A table with all the used textures is displayed.
If you change anything that affects the contents of this table then you must click the Refresh button to see the effect of the change in the table.
By clicking the name of a texture in the Name column the corresponding [[Asset]] is selected in the [[Content Browser]].
By clicking an [[Actor]], or [[Actor]] count, in the Actor(s) column the Actors using that texture in the [[Level]] are selected.

We can see the maximum and current dimensions, whatever that means.


# Optimization View Modes

There are a few [[View Mode|View Modes]] that can help with verifying that texture streaming works as intended and that the appropriate mip levels are being loaded.
These are found under [[View Mode]] > Optimization Viewmodes > Texture Streaming Accuracy.


## Required Texture Resolution

This [[View Mode]] will render textured objects in a color indicating the current texel to pixel ratio for that texture.
We must select an object to inspect, and then which texture on that object from a drop-down that shows up at the top of the [[Level Viewport]].

White means that the target mip level is loaded.
Yellow or red means that the mip level is too small, i.e. we may see blurry textures.
Teal and green means that the mip level is too high, i.e. we are wasting VRAM and may see shimmering textures.
Gray means that the object is not the selected one, if everything is gray then select an object to inspect by clicking it in the [[Level Viewport]] or the [[Outliner]].

A suggested workflow is to select an object, select a texture, and fly around the scene looking at the object from various angles and distances that we expect the user will also see the object from.
It if remains green throughout then we can lower the Maximum Texture Size for that texture by one power-of-two step, i.e. cut both the X and the Y dimension in half in [[Texture Editor]] > Details panel > Compression > Advanced > Maximum Texture Size.
Look at the object again and repeat until white.
If the object is red and there is a Maximum Texture Size set then double it until white or you reach the size of the source texture.
At that point you need to get a higher resolution texture or simply live with the blurriness.
As always, this process should be guided by artistic intent and technical limitations, but it's a decent starting point.
(
Is there a way to automate this?
Enable a Auto Size Textures tool, select the object, and move around the scene.
The tool should record the maximum texture size requested from the texture and then set the Maximum Texture Size to that value.
)

It is quite tedious and time consuming to iterate over all objects and all textures in that object.
Is there a way to ask the engine which texture are the most over-sampled right now?
How do I even know that I should start looking at the Required Texture Resolution [[View Mode]]?
Is it something someone has to do from time to time, just to be safe?
Sounds like a terrible waste of time.


# Console Variables

Unreal Engine allocates a limited amount of VRAM for texture streaming.
If we add too many different texture-using assets to our scene then we will fill that amount.
There is a [[Console Variable]] to control the size of the pool.
```
r.Streaming.PoolSize
```
This measures the size of the texture streaming pool in MiB.
By default it is 1000 (I think).
If you have VRAM to spare then you can increase it.
```
r.Streaming.PoolSize 3000
```

The limit can be disabled by setting it to zero.
Watch out for crashes due to out-of-memory errors.

We can lock the pool size by setting `r.Streaming.UseFixedPoolSize` to 1.
Not sure what this actually means.

# References

- [_Managing the Texture Streaming Pool | Tips & Tricks | Unreal Engine_ by Unreal Engine @ youtube.com, 2022](https://youtu.be/uk3W8Zhahdg)
- [_Texture Streaming_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/5.0/en-US/texture-streaming-in-unreal-engine/)

