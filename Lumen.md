Lumen is Unreal Engine 5's new dynamic global illumination lighting system.

A [[Light Sources]] may have a Indirect Lighting Intensity setting in the Details panel.
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

