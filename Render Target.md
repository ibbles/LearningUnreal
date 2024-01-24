A Render Target is a [[Texture]] that a [[Material]] can not only read from but also write to.
A Render Target is an asset that lives in the [[Content Browser]].
Created with [[Content Browser]] > right-click > Materials & Textures > Render Target.
A render target has a resolution, set from the [[Details Panel]] when opening the Render Target.

They are read in the same way as any other [[Texture]]:
In a [[Material]] create a [[Texture Sample]] node and set the source texture in the [[Details Panel]] to your Render Target asset.

# Bake Material To Texture

A Render Target can be used to procedurally generate a [[Texture]] holding the output of one evaluation of an expensive [[Material]].
When done a second [[Material]] can recreate the look of the expensive [[Material]] very cheaply by sampling the Render Target.

Create a new [[Blueprint]] [[Editor Utility Class]] by right-click in the [[Content Browser]] and select (TODO fill in here).
These Blueprint classes are run when double-clicked, so to edit it instead right-click and select Edit Blueprint.
Create a new [[Custom Event]] named Bake Material To Render Target and enable [[Details Panel]] > Graph > Call In Editor.
Call the Draw Material To Render Target function, which I believe is an engine built-in.
Create two variables:
- One of type Render Target.
- One of type [[Material Interface]].
Enable [[Details Panel]] > Variable > Expose On Spawn on both of them.
Double-click your [[Editor Utility Class]] to open a window with a button to invoke the Bake Material To Render Target [[Custom Event]] and a [[Property Widget]] each for the two variables.
Set the Render Target variable to the Render Target [[Asset]] to write to and the [[Material Interface]] variable to the [[Material]] that should be rendered.
Click the Bake Material To Render Target button, the [[Material]] will be evaluated and the result written to the Render Target.

I'm not sure how the [[Material]] is rendered.
With what [[Mesh]]? Just a quad? What size? What values will vertex attributes have?
With what [[Camera]] settings? Does that matter?
What will scene dependent nodes such as World Location or time return?

The Render Target is created with an alpha channel, so if you open the Render Target it will look empty until the alpha channel is disabled from the View menu in the top-left corner of the viewport.

The Render Target can be converted to a regular [[Texture]] by [[Content Browser]] > right-click > Create Static Texture.
Not sure what the advantage to this is, is a Render Targets more expensive to sample from than a regular [[Texture]]?
The alpha channel will be included in the generated [[Texture]], so if you open it in the [[Texture Editor]] you may need to disable the alpha channel from the View drop-down in the top-left corner of the viewport.

In the original [[Material]], or a different one, create a [[Texture Sample]] node that reads from the generated Render Target or [[Texture]].
The final look may not be exactly the same as the original [[Material]], not sure why.

The [[Content Examples]] project contains a level named Blueprint Render To Target that demonstrates advanced Render Target techinques.


# Get Camera View

A Render Target can be used to extract the view a [[Camera]] sees.
Can be used for example to create a security camera and a monitor displaying what the security camera sees.
Can be used to create very precise mirrors.

To render a [[Camera]] view to a Render Target create a Scene Capture 2D [[Actor]] or [[Component]].

The render resolution of the render from the camera's point of view matches the resolution of the Render Target.
A high resolution can have a significant impact on performance since we are rendering the entire scene again.
Reduce the rendering rate of the extra camera, or only update it when necessary based on gameplay logic, to get some performance back.
(
I don't know how to set a lower rendering quality for these extra cameras.
What quality control settings do we have access to?
)

# References

- [_Materials Master Learning_ > _Render Targets_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/7bn/render-targets)