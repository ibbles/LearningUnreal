A Runtime Virtual Texture is a [[Texture]] that can be written to from a [[Material]].
(
There must be more to it.
We can write to a [[Render Target]] as well.
The "virtual" part must mean something.
)

Must be enabled with [[Project Settings]] > Engine > Rendering > Virtual Textures > Enable Virtual Texture Support.


# Creating A Runtime Virtual Texture

A Runtime Virtual Texture is an [[Asset]] that lives in the [[Content Browser]].
Create a new Runtime Virtual Texture by [[Content Browser]] > right-click > Textures > Runtime Virtual Texture.

A Runtime Virtual Texture can have various types of content.
- Base Color
- Base Color, Normal, Roughness
- Base Color, Normal , Roughness, Specular
- World Height

A Runtime Virtual Texture exists within a volume of the [[Level]].
That volume is defined by placing and Runtime Virtual Texture Volume [[Actor]].
Set the Location and Scale of the Runtime Virtual Texture Volume to cover what you need it to.

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

# References

- [_# Virtual Texturing | Live from HQ | Inside Unreal_, by Epic Games @ youtube.com](https://www.youtube.com/watch?v=fhoZ2qMAfa4)
- [_Using Virtual Heightfield Mesh For Landscape Displacement In Unreal Engine 5 (Updated)_, by BuildGamesWithJon @ youtube.com](https://www.youtube.com/watch?v=H4jzMsiBkYg)

