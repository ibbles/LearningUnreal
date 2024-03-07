A Blend Material is a type of [[Material]] that contains multiple Materials that can be blended between.

It is common to use [[Vertex Color]] and [[Mesh Paint Editing Editor Mode Vertex Color]] to chose which material should be applied where by letting each channel of the vertex color control the blend strength of one of the blended materials.
Another way is to use a [[Perlin Noise]] node to get a random distribution of materials.
This is good when the goal is to break up texture repetition.
See also [[Breaking up Landscape Texture Tiling]].

There are multiple ways to do the blending.

One way is to let the [[Quixel Bridge]] plugin create the entire material, using [[Quixel Megascans Surfaces Material Blend]].

Another is to directly blend texture samples with a `lerp` mode.

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


# References

- [_Vertex Painting in Unreal Engine 4 w/ Javier Perez | NVIDIA Studio Sessions_ by NVIDIA Studio @ youtube.com](https://www.youtube.com/watch?v=D1XhB_DRiEY)

