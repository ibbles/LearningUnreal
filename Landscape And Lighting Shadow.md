#Landscape
#Lighting

# Fixing Shadow Artifacts During Sculpting
A newly created [[Landscape]] in a default level will have [[Shadows|Shadow]] artifacts around the Landscape Components until lighting has been built.
To make [[Sculpt a Landscape|Sculpting]] easier we can tweak the lighting settings to get rid of them.

## Directional Light

Select the [[Directional Light]], named Light Source, and switch [[Mobility]] from Stationary to Movable.
The shadow artifacts will disappear.
It can be changed back to Stationary again, the artifacts will not reappear.

## Sky Light

In the default level there is a Sky Light with Mobility set to Static.
Set this to Stationary to have ambient lighting in areas in shadow from the Directional Light while sculpting.


# Increasing Shadow-On-Landscape

When using [[Static Lighting]] Unreal will pre-compute [[Shadows]] from objects onto the [[Landscape]].
The resolution of the static lighting can be increased with [[World Outliner]] > [[Landscape]] > Details panel > Lighting > Static Lighting Resolution.
I don't know what this does.
What is a good value?
What are the memory and performance implications?
Is this a [[Lightmap]] resolution?
The unit of the value is texels per quad side, i.e. there will be `StaticLightingResolution^2` texels per quad.
This setting might be unused if the [[Light Source]] is using [[Cascaded Shadow Maps]].

[_How to fix Shadows.. And large resolution on foliage UE4_ - Static Light Resolution, by Batnobie X @ youtube.com 2021](https://youtu.be/w2FKHkW1XL8?t=43)

Shadows on [[Landscape]] is also affected by the [[Cascaded Shadow Maps]] settings on the [[Light Source|Light Source]].
Perhaps it must be a [[Directional Light]], not sure.
