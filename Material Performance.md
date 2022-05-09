[[Material]]
[[Material Editor]]

If you need [[Blend Mode]] to be [[Translucency|Translucent]] try to keep the [[Shading Model]] set to Unlit.
If it must be Lit, prefer to use Details panel > Translucency > Lighting Mode set to Volumetric NonDirectional.
Will produce blurry received shadows, i.e, shadows on the translucent object from the environment.
Can be made less blurry by increasing the `r.TranslucencyLightingVolumeDim` [[CVars|CVar]], default 64 try increasing to 256 or so.
The instruction count on Translucent materials is more important than on regular materials, keep it low.

Translucency can be faked by setting the [[Blend Mode]] to Masked and setting Details panel > Material > Dither Opacity Mask.
Will render faster.
Pass in Opacity to the Opacity Mask input pin on the Material Output Node .

Forward + reflections is very expensive.
Not sure what this means.
By "forward" does he mean not the deferred rendering pipeline?
By "reflections", does he mean high specular and low roughness?
[Rendering Kickstart - Rendering - Advice @ learn.unrealengine.com](https://learn.unrealengine.com/course/3537481/module/6853946)
