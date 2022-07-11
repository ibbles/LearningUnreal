An emissive [[Material]] are materials that is glowing.
Legacy rendering systems implemented emissiveness by always rendering the Material's base color, regardless of the light/shadow conditions.
A limitations of this approach is that an emissive material can't illuminate things around it.

With [[Lumen]] emissive materials can illuminate surrounding objects, with some caveats.
The light will be low-quality, meaning that it will be splotchy and uneven, and it will disappear with increased distance from the camera.

To create an emissive material, in the [[Material Editor]], plug a non-zero color into the Emissive Color input pin of the material output node.

# References
- [_Lighting in Unreal Engine 5 for Beginners_ by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=1394)

