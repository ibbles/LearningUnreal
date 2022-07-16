Enable [[Project Settings]] > Engine > Rendering > Hardware Ray Tracing >
- Support Hardware Ray Tracing.
- Ray Traced Shadows.
- Ray Traced Skylight.

Per light, Details panel > Light > Advanced > Cast Ray Traced [[Shadows]].
- Enabled: Use ray tracing when generating shadows for this light.
- Disabled: Do not use ray tracing, use [[Project Settings]] > Engine > Rendering
- Use Project Setting.

Mine is stuck on Use Project Setting, the drop-down is grayed out and non-interactible.
Why? Is there another setting somewhere that must be enabled?
Is it because I'm on Linux, and Unreal Engine 5.0 doesn't yet support Ray Tracing on Vulkan?

The quality of per-light ray tracing is set with Light > Details panel > Ray Tracing > Samples Per Pixel property.


# Translucency

Ray traced [[Translucency]] can be enabled from a [[Post Process Volume]].
In Details panel > Rendering Features > Translucency > Type select Ray Tracing.
The alternative is Raster.

# Settings Summary

## Project Settings

[[Project Settings]] > Engine > Rendering > Hardware Ray Tracing
- Support Hardware Ray Tracing.
- Ray Traced Shadows.
- Ray Traced Skylight.


## Post Process Volume

Details panel > Rendering Features > Translucency > Type set to Ray Tracing or Raster

## Light

Details panel > Light > Advanced > Cast Ray Traced Shadows.

# References

- [_Hardware Ray Tracing_, by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/5.0/en-US/hardware-ray-tracing-in-unreal-engine/)