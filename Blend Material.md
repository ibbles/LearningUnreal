A Blend Material is a type of [[Material]] that contains multiple Materials that can be blended between.

It is common to use [[Vertex Color]] and [[Mesh Paint Editing Editor Mode Vertex Color]] to chose which material should be applied where by letting each channel of the vertex color control the blend strength of one of the blended materials.
Another way is to use a [[Perlin Noise]] node to get a random distribution of materials.
This is good when the goal is to break up texture repetition.
Another way is to use a mask [[Texture]] to control which [[Material]] goes where.
See also [[Breaking up Landscape Texture Tiling]].

There are multiple ways to do the blending.

One way is to let the [[Quixel Bridge]] plugin create the entire material, using [[Quixel Megascans Surfaces Material Blend]].

Another is to directly blend texture samples with a `lerp` mode.

Another is to use a Blend Material Attributes node.
See below, and [[Material Attributes]].

It is common to have separate sets of [[Material Parameter|Material Parameters]], such as tiling, for the two [[Material]] layers.

Creating a [[Material Function]] for each [[Material]] you may want to include in a [[Blend Material]] makes reuse easier.

# Height Lerp

Another is to use a Height Lerp node along with a height map.
In this way we can get material blending that first fills in the cracks and crevices of one material with another, such as snow or dust in a cliff face, or paint or mold in a brick wall.
The Height Lerp node has five inputs:
- A: A [[Texture Sample]], or any other data, for the first material.
- B: A [[Texture Sample]], or any other data, for the second material.
- Transition Phase Lerp Alpha: How much of one or the other material we want at this location. Often taken from [[Vertex Color]] or possibly a [[Texture Sample]]. This is what gives artistic control over how much of the second material, the dust, snow, paint, mold, etc, there should be in different locations of the mesh.
- Height Texture: Control where the transition point is for this location, i.e. what the Transition Phase Lerp Alpha would need to be to switch from one of the materials to the other. This should be a grayscale height map for the base material, e.g. the cliff face or the brick wall.
- Contrast: Not entirely sure. Controls how blending in the seems is done, whether the transition is sharp or smooth over some distance, I think.

I think Height Lerp does something like the following, ignoring Contrast:
```cpp
template <typename T>
T HeightLerp(T A, T B, float TransitionPhaseLerpAlpha, float HeightTexture)
{
	return TransitionPhaseLerpAlpha > HeightTexture ? A : B;
}
```


# Blending Material Attributes

When creating a Blend Material it may help to create complete separate [[Material Attributes]] and blend those rather than blending the individual [[Material Output Node]] inputs.
Create a Make Material Attributes node for each layer and populate those as you would a regular [[Material]].
Then use a Blend Material Attributes (Or so I though, see below.) node to produce the final [[Material Attributes]] that goes into the [[Material Output Node]].
There is a whole bunch of Mat Layer Blend nodes that does blending in various ways.
Experimentation recommended.
(The _An In-Depth Look at Environment Artist Based Tools_ > _Blending Materials_ example did not create a Blend Material Attributes node, instead it created a May Layer Blend Standard node. I don't know what the difference is.)
The Mat Layer Blend Standard node takes an Alpha input that controls how much of either [[Material]] should be passed on through.
The Alpha input can be connected to a mask [[Texture]] that controls which [[Material]] should go where, a [[Perlin Noise|noise node]] of some kind to get a random blotchy pattern, a [[Height Map]] to get one [[Material]] crevices and another on top, [[Vertex Color]] to make it possible to paint the [[Material]] on to the [[Mesh]], the world-space normal to get one material on top of an object and another material on the sides, or any other way you can think of.
It is often a good idea to clamp the Alpha value to be between 0.0 and 1.0.

To make it possible to connect a [[Material Attributes]] node to the [[Material Output Node]] we need to enable [[Details Panel]] > Material  Use Material Attributes.
This will collapse the [[Material Output Node]] input  pins down to a single [[Material Attributes]] input pin.

# Blending With Vertex Color

Create two Make [[Material Attributes]] nodes and populate their inputs to produce the layers you want.
Create a Mat Layer Blend Standard node and connect the two Make [[Material Attributes]] nodes to the Base Material and Top Material input pins.
Connect the Mat Layer Blend Standard output pin to the [[Material Output Node]].
Create a [[Vertex Color]] node and connect one of the channels to the Alpha input on the Mat Layer Blend Node.

Assign the [[Material]] to a [[Static Mesh]] in the [[Level Viewport]].
Switch to the [[Vertex Paint]] [[Editor Mode]].
Set Mesh Paint panel > Visualization > Color View Mode to Off.
Set Mesh Paint panel > Vertex Painting > Paint Color to white and Erase Color to black.
Set Mesh Paint panel > Color Painting > Channels to Red.
Paint and / or erase, i.e. click-drag or Shift + click-drag over the [[Static Mesh]].
You will paint the different layers onto the mesh.

## Vertex Color And Height Lerp

It is possible to combine Vertex Color based blending with Height Lerp based blending.
Then we get the effect where in the transition area we, for example, transition to the new material in the lower parts first and then the higher parts.
Imagine a dirt layer and a cobblestone layer and the dirt shows up in the gaps between the stones first as we move from full-cobblestone to full-dirt.
In this case we don't use the A and B inputs on the Height Lerp node.
Instead we connect the Alpha output of Height Lerp into the Alpha input of the Mat Layer Blend Standard node.
Connect the Vertex Color channel to Transition Phase.
Connect the [[Height Map]] [[Texture Sample]] to Height Texture.
You may need a One Minus node as well, if the transition happens at the top and you want the bottom, or vice versa.
Connect a [[Material Parameter]] to Contrast if you want.

Advantage of this technique is that you get layer blending effect at a resolution higher than the resolution of the [[Static Mesh]] since we get per-texel variations within a single triangle.
This makes the blotchy  layer transitions that a pure [[Vertex Color]] based blending suffer from less visible.

## Adding A Third Layer

Adding more layers is basically just a repeat of the previous steps, using the output of the Mat Layer Blend Standard node as one of the two layers to blend.
Create a new Mat Layer Blend Standard node.
Connect the output of the first Mat Layer Blend Standard to the Base Material input on the second Mat Layer Blend Standard node.
Connect output of the third Make [[Material Attributes]] node to the Top Layer of the second Mat Layer Blend Standard node.
Create a second Height Lerp node and connect another channel output from the [[Vertex Color]] node to the Transition Phase input of the second Height Lerp node.
Connect the third layer's [[Height Map]] value into  the Height Texture input on the Height Lerp node.

## Masking Small Objects

Some textures represent small objects scattered over the texture.
Such textures have an alpha channel that have white where the objects are and black where they are not.
To use the texture alpha channel multiply it with the Alpha output of the Height Lerp node before passing it to the Alpha input on the Mat Layer Blend Standard node.
The effect of this is that the layer is  only considered where there is an object, otherwise the other layer is used.
The black color in the alpha texture channel forces the blend Alpha to zero.

It can be difficult to prevent broken objects since the scale of the objects are typically smaller than the resolution of the [[Static Mesh]].
This means that we get transitions between vertices that we have little control over, transitions that may happen right across one of the objects.
Not sure what to do about that.

# References

- [_Vertex Painting in Unreal Engine 4 w/ Javier Perez | NVIDIA Studio Sessions_ by NVIDIA Studio @ youtube.com](https://www.youtube.com/watch?v=D1XhB_DRiEY)
- [_An In-Depth Look at Environment Artist Based Tools_ > _Blending Materials_ by Epic Online Learning @ dev.epicgames.com/courses 2021 UE4.25](https://dev.epicgames.com/community/learning/courses/3G/unreal-engine-an-in-depth-look-at-environment-artist-based-tools/07y/unreal-engine-blending-materials)
