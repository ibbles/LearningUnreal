#Rendering

![](Images/Rendering_Pipeline_Overview.png)


The rendering pipeline is three frames deep, meaning that the pixels on screen is three frames old.
- Position Updates
	- Physics, animation, gameplay logic, etc.
- [[Occlusion Visibility Process]]
- GPU Rendering
	- [[Early Z Pass]]
	- [[Base Pass]]

After the [[Occlusion Visibility Process]] we end up with a list of things to render.

