A Virtual Texture is a [[Texture]] that supports a type of texture streaming added in Unreal Engine 4.23.
Splits the texture data into fixed-sized tiles, such as 128x128 pixels (Or texels? Are these in texture space or screen space?).
What tiles to stream in is based on what is visible on screen and what tiles are required to texture those on-screen elements.
Has a mip-map-like structure using a  page table for each tile so different quality levels can be loaded for different parts of the textured objects.
As you move closer to the object more and more higher-resolution tiles are streamed in, replacing the lower-resolution tiles.
Tiles outside of the view frustum or occluded by other geometry is not loaded.

Must be enabled in [[Project Settings]] > Engine > Rendering > Virtual Textures > Enable Virtual Texture Support.
Must be enabled per-texture in [[Texture Editor]] > Details panel > Texture > Virtual Texture Streaming.
This changes the Method in the info panel at the top of the Details panel from Streamed to Virtual Streamed.
Sampling a Virtual Texture in a [[Material]] is a bit different from sampling a regular [[Texture]], the Texture Sample node should have a different Sampler Type, so if your [[Texture]] is already used by a [[Material]] then instead of enabling Virtual Texture Streaming from the [[Texture Editor]] prefer to do [[Content Browser]] > Texture Asset(s) > right-click > Convert To Virtual Texture.
This will update all [[Material|Materials]] that use that [[Texture]].
It will also find all other [[Texture|Textures]] sampled by those [[Material|Materials]] and convert those to Virtual Textures in the same way, which may find additional [[Material|Materials]] top update, which may find additional Textures to convert, and so on recursively.
The conversion tool can be run with multiple [[Texture|Textures]] selected in the [[Content Browser]].


# Sampling A Virtual Texture

Virtual Textures are used in the same way as regular textures in a [[Material]], create a Texture Sample node and assign a [[Texture]] with Virtual Texture Streaming enabled to Texture Sample > Details panel > Material Expression Texture Base > Texture.
A VT icon will be displayed in the texture preview to show that this is a Virtual Texture.
The Sampler Type for Texture Sample node that is bound to a Virtual Texture starts with Virtual, e.g Virtual Color, Virtual Normal, etc.
Sampling a Virtual Texture is more expensive than sampling a regular [[Texture]].
If you sample multiple textures with the same texture coordinate, or UV, then they form a Virtual Texture Stack.
This can amortize the extra cost of Virtual Texture Sample over regular [[Texture]] samples.
The number of Virtual Texture Stacks is shown in the Stats panel in the [[Material Editor]], aim to keep it as low as possible.

If you create a [[Material Instance]] from a [[Material]] that samples a Virtual Texture and in the [[Material Instance]] replace the [[Texture]] with another, then the new [[Texture]] must also be a Virtual Texture.
The reverse is also true, if the parent [[Material]] uses a regular [[Texture]] then the [[Material Instance]] must also use a regular [[Texture]], not a Virtual Texture.


# Writing A Virtual Texture

It is possible to write to a Virtual Texture, but not to a regular Virtual Texture [[Asset]].
A writable Virtual Texture is called [[Runtime Virtual Texture]].


# Console Variables

There is a family of [[Console Variable|Console Variables]] related to virtual textures, all prefixed with `r.VT`.
- `r.VT.Flush`: Not sure yet.
- `r.VT.Borders`: Render the tile borders in the viewport. Move closer to the textured object see more and more tiles being loaded in.


# References

- [_Virtual Texturing | Live from HQ | Inside Unreal_, by Unreal Engine @ youtube.com. 2019](https://www.youtube.com/watch?v=fhoZ2qMAfa4)

