[[Ambient Occlusion]] is a way to fake indirect lighting and/or small scale occlusion on a lit object.
It works by estimating how much light is obstructed per pixel (?) by nearby geometry and reduce the light intensity by that amount.
This produces darkened corners and crevices.
It give more definition to the mesh.

Distance Field Ambient Occlusion is provided by [[Sky Light]].
It only works if the [[Sky Light]] its [[Light Mobility]] set to movable.
The settings for this is in [[Sky Light]] > Details panel > Distance Field Ambient Occlusion.

- Occlusion Max Distance: The distance from a shaded point that surrounding geometry contribute to [[Ambient Occlusion]]. Should be set to a value that matches the scale of irregularities in the geometry. Experimentation needed.

When configuring Distance Field Ambient Occlusion on a [[Sky Light]] it can help to turn off all other [[Light Source|Light Sources]].


Another form of [[Ambient Occlusion]] is [[Baked Ambient Occlusion]].
