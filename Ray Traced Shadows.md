Must be enabled per [[Light Source]] in Details panel > Light > Cast Ray Tracing Shadows.

# Culling Distant Instanced Geometry And Foliage

By default Unreal Engine will cull far-away [[Instanced Static Mesh]] geometry, such as foliage.
To keep them, so they cast shadows even in the distance, disable the `r.Raytracing.Geometry.InstancedStaticMeshes.Culling` [[Console Variable]] by setting it to 0.
This will come with a performance cost.

[_Troubleshooting FOLIAGE issues in Unreal Engine_ - Disappearing Shadows, by William Faucher @ youtube.com](https://youtu.be/Ar3vvygirLU?t=75)



# Weird Shadows On Wind-Affected Foliage

(
What is described below is the big-hammer solution.
It comes with a big performance penaly.

An alternative solution that may work in some cases is to:
- Enable [[Light Source]] > [[Details Panel]] > ??? > Evaluate World Position Offset.
- Set the `raytracing.nanite.mode` [[Console Variable]] to `1`.
)

The [[Quixel Megascans]] foliage assets come with a wind effect.
Enabling this in combination with [[Light Source|Light Source]] > Details panel > Light > Cast Ray Tracing Shadows can cause shadowing artifacts where the wind itself becomes a shadow.
It looks like darker blobs moving across the grass or showing up on tree trunks.

On a regular [[Static Mesh]] this can be fixed by enabling Details panel > Rendering > Ray Tracing > Evaluate World Position Offset.
This does not work for [[Foliage]] even though the setting is in that Details panel as well.
Possibly a bug fixed at or after Unreal Engine 5.0.

A workaround for foliage is to not use actual foliage, but instead create a [[Blueprint Class]], inheriting from [[Actor]], that contains a single [[Static Mesh Component]] set to the foliage [[Static Mesh Asset]].
On the [[Static Mesh Component]] enable Details panel > Rendering > Ray Tracing > Evaluate World Position Offset.
Create an Actor Foliage [[Asset]] with Content Browser > right-click > Foliage > [[Actor Foliage]].
[[Actor Foliage]] is a type of [[Foliage Type]].
In the [[Actor Foliage]] [[Asset]] set Details panel > Actor > Actor Class to the [[Blueprint Class]] we just created.
Add the [[Actor Foliage]] to the Foliage Type list in the [[Foliage Mode]].
Paint with the new [[Foliage Type]].
Since these aren't actually foliage, their full-fledged Actors, the Evaluate World Position Offset setting works and the shadow artifacts doesn't happen.
But it comes with a high performance cost.

[_Troubleshooting FOLIAGE issues in Unreal Engine_ - , by William Faucher @ youtube.com](https://youtu.be/Ar3vvygirLU?t=142)


# References

- [_Troubleshooting FOLIAGE issues in Unreal Engine_, by William Faucher @ youtube.com](https://www.youtube.com/watch?v=Ar3vvygirLU)

