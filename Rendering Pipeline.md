#Rendering

![](Images/Rendering_Pipeline_Overview.jpg)


The rendering pipeline is three frames deep, meaning that the pixels on screen is three frames old.
- Position Updates
	- Physics, animation, gameplay logic, etc.
- [[Occlusion Visibility Process]]
- GPU Rendering
	- [[Early Z Pass]]
	- [[Base Pass]]

After the [[Occlusion Visibility Process]] we end up with a list of things to render.


# Rendering Hardware Interface - RHI

Unreal Engine supports multiple 


# References

- [_Graphics Programming Overview_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/5.0/en-US/graphics-programming-overview-for-unreal-engine/)

