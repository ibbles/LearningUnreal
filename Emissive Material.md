An emissive [[Material]] are materials that is glowing.
To create an emissive material, in the [[Material Editor]], plug a non-zero color into the Emissive Color input pin of the material output node.

Combined with post-processing an emissive material will create a blooming effect,
leaking light into surrounding pixels.
The blooming can be controlled with a [[Post Process Volume]].
[[Details Panel]] > Lens > Bloom > {Intensity, Threshold}.
Intensity control how far an emissive surface will leak light.
Threshold control how strong the emissive color must be before it starts leaking.

Legacy rendering systems implemented emissiveness by always rendering the Material's base color, regardless of the light/shadow conditions.
A limitations of this approach is that an emissive material can't illuminate things around it.

With [[Lumen]] emissive materials can illuminate surrounding objects, with some caveats.
The light will be low-quality, meaning that it will be splotchy and uneven, and it will disappear with increased distance from the camera.
Small emissive light-sources are extra low-quality, so keep emissive areas large when using [[Lumen]].



# References
- [_Lighting in Unreal Engine 5 for Beginners_ by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=1394)

