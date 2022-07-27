Lumen is Unreal Engine 5's new dynamic global illumination lighting system.

A [[Light Source]] may have a Indirect Lighting Intensity setting in the Details panel.
This setting controls how much that light source contribute to the global illumination around objects directly illuminated by the light source.

# Requirements

On Windows, set Project Settings > Platforms > Windows > Default RHI to DirectX 12,
and enable DirectX 11 & 12 (SM5).

On Linux enable, set Project Setting > Platforms > Linux > Vulkan Desktop (SM5).

In Project Settings > Engine > Rendering
- Set Global Illumination > Dynamic Global Illumination Method to Lumen.
- Enable Lumen > Use Hardware Ray Tracing When Available.
- Set Lumen > Ray Lighting Mode to Surface Cache.
- Set Lumen > Software Ray Tracing Mode to Detail Tracing.
- Set Shadows > Shadow Map Method to Virtual Shadow Maps.
- Enable Hardware Ray Tracing > Support Hardware Ray Tracing.

(
Not sure if the settings above are suitable for real-time rendering or more for cinematics.
)


# Hardware Ray Tracing

Lumen can use hardware ray tracing, if supported by the hardware.
Ray tracing is supported by NVIDIA RTX card, AMD Radeon 6000 series and up, and Intel ARC.
Hardware ray tracing support for Lumen is enabled with [[Project Settings]] > Engine > Rendering > Lumen > User Hardware Ray Tracing When Available.

With hardware ray tracing enabled Lumen reflections will include off-screen Mobility = Movable objects.
(
I think mobility is the determining factor.
)
Make sure [[Project Settings]] > Engine > Rendering > Reflections > Reflection Method is set to Lumen.

There are a bunch of other Ray Tracing related settings in [[Project Settings]] > Engine > Rendering > Hardware Ray Tracing.
I don't think they are related to Lumen.
Not sure under what conditions they apply.

On a [[Post Process Volume]] we can tweak some more Lumen settings.


# Flickering / Jittering / Crawling Indirect Lighting

Sometimes Lumen causes flickering / jittering / crawling light in indirectly illuminated parts of the level.
This happens because Lumen is an approximate light propagation simulation algorithm.
We can reduce it by increasing the Lumen quality settings.
In a Post Process Volume, increase Global Illumination > Lumen Global Illumination >
- Lumen Scene Lighting Quality
- Final Gather Quality
- Advanced > Final Gather Lighting Update Speed

# References

- [_Lighting in Unreal Engine 5 for Beginners_, by William Faucher @ youtube.com](https://www.youtube.com/watch?v=fSbBsXbjxPo)
- [_Lighting in Unreal Engine 5 for Beginners_ - Fixing crawling indirect lighting, by William Faucher @ youtube.com](https://youtu.be/fSbBsXbjxPo?t=2185)
- [_Unreal Engine 5 New Lumen Hardware Ray tracing Reflections_, by JSFILMZ @ youtube.com](https://www.youtube.com/watch?v=rQ0zJFgdqHE)
