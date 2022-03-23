# Early Z Pass
First is the Early Z Pass that renders the Scene Depth buffer.
This helps reduce pixel overdraw.
I assume this works by doing early-out in the fragment shader when a fragment is behind the Scene Depth at that pixel.

There are settings in Project Settings > Engine > Rendering > Optimizations > Early Z-pass.
Rarely changed, the default is often good.