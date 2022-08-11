[[Ambient Occlusion]] is a way to fake indirect lighting and/or small scale occlusion on a lit object.
It works by estimating how much light is obstructed per pixel (?) by nearby geometry and reduce the light intensity by that amount.
This produces darkened corners and crevices.
It give more definition to the mesh.

Unreal Engine has:
- [[Distance Field Ambient Occlusion]].
	- This is a feature on the [[Sky Light]].
- [[Baked Ambient Occlusion]]
	- This is generated at bake time by [[Lightmass]], only for static lights and objects.


