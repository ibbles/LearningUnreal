A Blend Material uses the [[Vertex Color|Vertex Colors]] of a mesh to blend between multiple layers of a [[Material]].
A Quixel Megascans Surface Material Blend uses [[Quixel Megascans Surfaces]] as its layers.
This means that we can paint such surface materials onto a [[Static Mesh]].
The resolution of our painting is limited to the vertex density of the mesh, since we paint on vertices, not a texture.


# Creating A Material Blend

To create a material blend:
- In a [[Content Browser]] select three [[Material Instance|Material Instance]] imported using [[Quixel Bridge]].
	- The order they are selected determines which channel of the [[Vertex Color]] that [[Material Instance]] map to.
- Open Quixel Bridge.
- Select any asset.
- Click the settings button next to the quality selection drop-down.
	- This will open the Megascans settings window.
- Click the Material Blend Settings > Create Material Blend button.

This creates a folder named `BlendMaterials` in the root Content folder.
Inside that folder is a newly created [[Material Instance]].
Assign that material to a [[Static Mesh]] by dragging it from the Content Browser onto the mesh in the viewport.
Activate the [[Mesh Paint Editing Editor Mode]].
In the Mesh Paint panel select the Colors tab and then the Paint tool.
Paint with red to increase the strength of the first material, green for the second material, and blue for the third material.


# Material Settings

The Material Blend [[Material Instance]] has a number of settings that control of the three materials are blended.
I don't know what they do yet.


# References

- [_How to use mesh paint editing tool in UE5 (Tutorial)_ by LoneWolf Studio @ youtube.com](https://www.youtube.com/watch?v=yWOYmhNoobU)



