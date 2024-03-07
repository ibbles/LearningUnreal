Unreal Engine 5 introduced [[Temporal Super Resolution]], an up-scaling / anti-aliasing method.
To work this algorithm needs motion vectors for everything on-screen.
This is handled automatically for Static Mesh and Skeletal Mesh, as long as [[World Position Offset]] isn't used, but as of Unreal Engine 5.1 multiple things don't yet support writing motion vectors.
If you have ghostly trails after objects then use the console to enable the Visualize Motion Blur flag:
```
ShowFlag.VisualizeMotionBlur 1
```
This will show you where the renderer is aware that objects are moving using yellow arrows.
If you see a moving object without yellow arrows then you know that motion vectors aren't being written and [[Temporal Super Resolution]] will fail to resolve that object correctly.
This is not always possible to fix.

One option to fix this is to not use a temporal anti-aliasing method.
Change [[Project Settings]] > Engine > Rendering > Default Settings > Anti-Aliasing Method to something other than Temporal Super-Resolution (TSR) or Temporal Anti-Aliasing (TAA).

Resolution scaling can also be a factor, increasing it closer to 100% may reduce the smearing / ghosting.
Set Top Menu Bar > Settings drop-down > Engine Scalability Settings > Screen Percentage to 100%.
This only effects Play-in-Editor sessions.

There are a lot of Screen Percentage settings in the Project Settings.
I don't know how these should be set.

There is also [[Editor Preferences]] > General Performance > Editor Viewport Screen Percentage.
Must enable Override Project's Manual Screen Percentage right next to it for this to be editable.

`r.TSR.ShadingRejection.Flickering 0`
No idea what this is or what it does. Seems to help sometimes, no idea during what circumstances.


# Static Mesh

If you move a Static Mesh by setting the Location part of the Transform directly, also set the Velocity [[Property]].


# World Position Offset

If the [[Material]] uses [[World Position Offset]], which is common for foliage, then the motion vectors may be wrong and e.g. branches swaying in the wind will appear with trails behind them.

Possible workaround: Set [[Project Settings]] > Engine > Rendering > Optimizations > Output Velocities Due To Vertex Deformation to On.

Possible workaround: Set [[Project Settings]] > Engine > Rendering > Velocity Pass to Write During Base Pass.


# Niagara

Change [[Project Settings]] > Plugins > Niagara > Renderer > Default Render Motion Vector Setting to Precise.
There is a similar setting in the [[Niagara Mesh Renderer]] module in a [[Niagara Emitter]] at Mesh Renderer > Rendering > Motion Vector Setting.
Prior to Unreal Engine 5.3, or thereabout, this setting was under Motion Blur.


# Text Render Component

On the [[Material]], if it is translucent, select the output node and enable Translucency > Output Depth And Velocity in the [[Details Panel]].


# Instanced Static Mesh

When updating the transform of an instance make sure you also supply the previous transform.
Use the [`UInstancedStaticMeshComponent::BatchUpdateInstancesTransforms`](https://docs.unrealengine.com/5.3/en-US/API/Runtime/Engine/Components/UInstancedStaticMeshComponent/BatchUpdateInsta-/3/) overload that takes a `NewInstancesPrevTransforms` parameter.

# References

- [_Anti-Aliasing problems_ by Old_Influence_8568 @reddit.com](https://www.reddit.com/r/unrealengine/comments/13opbgy/antialiasing_problems/)
