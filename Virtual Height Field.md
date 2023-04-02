Used with [[Landscape]].
(Can it also be used on regular [[Static Mesh]]?

A Virtual Height Field is .... (something about distance based tessellation, I think.)


# Virtual Textures

Requires that [[Project Settings]] > Engine > Rendering > Virtual Textures > Enable Virtual Texture Support is enabled.
In Unreal Engine 5.0 it also requires the Virtual Heightfield Mesh [[Plugin]].

Needs a [[Runtime Virtual Texture]] to store height / displacement data into.
Create one with [[Content Browser]] > right-click > Textures > Runtime Virtual Texture.
Runtime Virtual Textures used for a Virtual Height Field is typically prefixed with `VHM_`.
The Virtual Height Field requires two [[Runtime Virtual Texture|Runtime Virtual Textures]]:
- `VHM_Height`
	- Content: World Height.
- `VHM_Material`
	- Content: Base Color, Normal, Specular.

A Virtual Texture need a Runtime Virtual Texture Volume to define its bounds.
The Virtual Height Field will only work within this volume.
Each Runtime Virtual Texture needs its own Runtime Virtual Texture Volume [[Actor]].
A Virtual Texture Volume is associated with a Virtual Texture [[Asset]] with the Details panel > Virtual Texture > Virtual Texture property.
By convention, prefix the Actors' names with `RVTV_`, so
- `RVTV_Height`
- `RVTV_Material`
Set the Location and Scale of both Runtime Virtual Texture Volumes to match the size of the [[Landscape]].


# Writing Virtual Textures From Material

The [[Landscape]] must know that it will draw to those Virtual Textures.
This is set in Landscape > Details panel > Virtual Texture > Draw In Virtual Textures.
This is an array of Runtime Virtual Texture assets, which should be set to `VHM_Height` and `VHM_Material`.

We write to a [[Runtime Virtual Texture]] by adding a Runtime Virtual Texture Output node to the [[Material]] graph.
Pass Base Color (V3), Roughness (S), Normal (V3), and World Height (S) to the Runtime Virtual Texture Output.
Base Color, Roughness, and Normal are typically easy, just pass in the same as you pass to the [[Material Output Node]].
The World Height is the output of Absolute World Position masked to just B (Z axis) plus any displacement.

Height gets a bit complicated if the [[Material]] blends multiple [[Material Attributes]] since there is no member to hold World Height displacement anymore.
Prior to Unreal Engine 5.0 there was a Displacement input pin on the [[Material Output Node]], but it was removed.
If you have a free pin on you [[Material Attributes]], such as Specular, then you can hijack that to hold the displacement instead and then pass that from the blended [[Material Attributes]] to the Height input pin on the Runtime Virtual Texture node.
Remember to clear it before passing to the [[Material Output Node]].
(How?)
Ugly...
It may also be possible to use the World Position Offset attribute. Testing needed.

Multiply the displacement by a scalar parameter, since displacements are stored in 0..1 range but actual displacements are often longer than 1 cm, and then add to the masked Absolute World Position before passing to World Height in the Runtime Virtual Texture Output node.

If you select either of the two Runtime Virtual Heightfield Volumes you should see the shape of your sculpted Landscape in the Runtime Virtual Texture preview in the Details panel of `RVTV_Height` and the layout of you material blend in the Details panel of  `RVTV_Material`.


# Mesh Rendering

A Virtual Height Field is rendered with the Virtual Heightfield Mesh [[Actor]], so create one in the [[Level]].
Set the Location and Scale of the Virtual Height Field to the same values as for the two Virtual Texture Volumes.
The Virtual Heightfield Mesh must know the height of the height field at each point.
This is stored in the `VHM_Height` Runtime Virtual Texture but we need to pass the Runtime Virtual Texture Volume associated with `VHM_Height`, so set Details panel > Heightfield > Virtual Texture to `RVTV_Height`.
A Virtual Heightfield Mesh also needs a Min Max texture.
The Min Max texture can be generated automatically by clicking Virtual Height Field Mesh > Details panel > Heightfield Build > Build Min Max Texture > Build.
Give the newly created texture a name and location in the dialog that appears.

By default Virtual Heightfield Mesh has Details panel > Rendering > Actor Hidden In Editor enabled.
Don't know why.
Disable this to see the shape of the heightfield, with the regular gray checkerboard material.

We also need  to assign a [[Material]].
This is not done by assigning the `RVTV_Material` Runtime Virtual Texture in the Details panel, instead we need to set a regular [[Material]].
This cannot be the [[Material]] we use on the [[Landscape]], but it should be a [[Material]] that samples the `RVTV_Material` Runtime Virtual Texture.
Create a new [[Material]] named, for example, `M_VHM`.
In this [[Material]], create a Runtime Virtual Texture Sample node.
In the Details panel for the sample node set Virtual Texture to `VHM_Material`.
From the sample node to the [[Material Output Node]] connect Base Color, Roughness, and Normal.
Set Virtual Height Field Mesh > Details panel > Rendering > Material to `M_VHM`, the [[Material]] we just created.


# Limitations

The displacements created in the Virtual Heightfield Mesh is not visible to the CPU and therefore not considered when doing collision detection or feet placement.

One way to fix the feet placement is to access RVT Height in your Character Blueprint to get the rendered height at the foot location and pass that to an inverse kinematics engine.
I don't know how to do that.


# C++

TODO: Figure out how to update a runtime virtual texture from C++.


# References

- [_Using Virtual Heightfield Mesh For Landscape Displacement In Unreal Engine 5 (Updated)_, by BuildGamesWithJon. 2022-05-19, UE 5 @ youtube.com](https://www.youtube.com/watch?v=H4jzMsiBkYg)
- [_Landscape Displacement Using Virtual Height Mesh In Unreal Engine 5_, by BuildGamesWithJon. 2021-12-29, UE 5.0 @ youtube.com](https://www.youtube.com/watch?v=cV2YhjAiogY)
- [_Displacements on Unreal Engine 5(UE5) Landscapes with Virtual Heightfield Mesh_ by GDi4K @ youtube.com. 2021, UE 5.0](https://youtu.be/JzYVeIF89V0)
	- More of an overview than a tutorial, doesn't show all steps or describe all parts. Uses a plugin named Open Land, or something like that.



