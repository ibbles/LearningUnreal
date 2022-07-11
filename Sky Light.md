A [[Light]] that produces light from a dome around the scene based on a sky texture.
This texture may be called an HDRI, High Dynamic Range Image, because it has more light information than a regular 8-bit texture.
The sky texture can either be provided or created from the contents of the scene.
The Sky Light does not render the texture, you won't see the sky, but it will light objects in the scene.
It will also provide reflections in objects such as chrome balls, when using the [[Lumen]] lighting engine.

You can use the [[HDRI Backdrop]] Actor, which is a combination of a Sky Light and a sky dome mesh, to get both the light and the sky.

By default a Sky Light only uses the upper hemisphere of the HDRI texture, it is a SKY light after all.
We can enable the lower hemisphere as well with Sky Light > Details panel > Light > Advanced > Lower Hemisphere Is Solid Color > Disable.

# References

- [_Lighting in Unreal Engine 5 for Beginners_ - Sky Light by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=1284)
- [_Demystifying the Skylight Unreal Engine 4 & 5_ by William Faucher @ youtube.com](https://www.youtube.com/watch?v=BGoaPyfZlYg)

