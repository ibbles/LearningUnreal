A Blend Material uses the [[Vertex Color|Vertex Colors]] of a mesh to blend between three layers of a [[Material]].
The three layers are called base, middle, and top.
A Quixel Megascans Surface Material Blend uses [[Quixel Megascans Surfaces]] as its layers.
This means that we can paint such surface materials onto a [[Static Mesh]].
The resolution of our painting is limited to the vertex density of the mesh, since we paint on vertices, not a texture.


# Creating A Material Blend

(
These instructions are broken on Unreal Engine 5.2-5.3.
See https://www.youtube.com/watch?v=ugKr8Cx4ye8
In short, create the blend material with a single [[Material Instance]] selected in the [[Content Browser]] and then manually set the textures for the other two in the [[Material Instance Editor]].
)

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
Activate the [[Mesh Paint Editing Editor Mode Vertex Color]].
In the Mesh Paint panel select the Colors tab and then the Paint tool.
Paint with red to increase the strength of the first material, green for the second material, and blue for the third material.
(
This may be wrong.
It may be that no color is the first material, red is the second material, and green is the third.
The blue color is for puddles.
See the _UE4 Easy & Awesome Blend Materials - Tutorial - With Quixel Megascans_ reference below.
But it's backwards.
Painting black on a color channel enables that material, painting white removes it.
I'm confused.
)


# Puddles

The blue channel is used to create puddles.
In this context a puddle is an extra color that is blended on top of the painted material.


# Material Settings

The Material Blend [[Material Instance]] has a number of settings that control of the three materials are blended.
I don't know what all of them do yet.

## Blend Controls
The blend controls control how two layers are blended.
I believe it controls the blending between two vertices, but I'm not sure. Maybe it can affect at a vertex as well.
There is one set of controls for the base-to-middle transition and another for the middle-to-top.


# References

- [_How to use mesh paint editing tool in UE5 (Tutorial)_ by LoneWolf Studio @ youtube.com](https://www.youtube.com/watch?v=yWOYmhNoobU)
- [_UE4 Easy & Awesome Blend Materials - Tutorial - With Quixel Megascans_ by UE4 Mentor @ youtube.com](https://www.youtube.com/watch?v=FfC0aXgxtAc)

