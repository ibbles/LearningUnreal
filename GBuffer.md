The GBuffer form the backbone of the deferred renderer.
The GBuffer is the output of the [[Base Pass]].
It is various types of views/versions into the scene, each showing different aspects of the rendered objects.
They are textures showing the world from the view of the camera.
- World normals. The direction each surface is facing.
- Material parameters.
	- Specular.
	- Roughness.
	- Metallic.
- Base color. Also called diffuse color. (I think.)
- Some more stuff I don't know what it is.

The GBuffer can be visualized with Viewport > Lit > Buffer Visualization > Overview.

The deferred rendering lighting calculations use the GBuffer as input.
