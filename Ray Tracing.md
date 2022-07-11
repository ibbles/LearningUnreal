Enable Project Settings > Rendering > Hardware Ray Tracing >
- Support Hardware Ray Tracing.
- Ray Traced Shadows.
- Ray Traced Skylight.

Per light, Details panel > Light > Advanced > Cast Ray Traced Shadows.
- Enabled
- Disabled
- Use Project Setting.

Mine is stuck on Use Project Setting, the drop-down is grayed out and non-interactible.
Why? Is there another setting somewhere that must be enabled?
Is it because I'm on Linux, and Unreal Engine 5.0 doesn't yet support Ray Tracing on Vulkan?

The quality of per-light ray tracing is set with Light > Details panel > Ray Tracing > Samples Per Pixel property.
