The frame rate can be fixed in Project Settings > Engine > General Settings > Framerate [(1)](https://docs.unrealengine.com/4.27/en-US/TestingAndOptimization/PerformanceAndProfiling/SmoothFrameRate/).

If the framerate becomes capped do the following:
- Check t.MaxFPS
- Check r.VSync
- Check r.VSyncEditor
- Check r.Vulkan.DebugVSync
- Check Project Settings > Engine > General Setting > Framerate > Use Fixed Framerate.
- Disable window manager (KDE etc) compositing.
- Resize the Unreal Editor window.
	- (This used to be "Move Unreal Editor to the other monitor and move it back again.", but I think resize is sufficient. TODO: Test it.)

# References

- 1: [SmoothFrameRate @ docs.unrealengine.com](https://docs.unrealengine.com/4.27/en-US/TestingAndOptimization/PerformanceAndProfiling/SmoothFrameRate/)