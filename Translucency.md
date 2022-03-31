Challenging to to in real time.
Unreal Engine has many variants.

Enabled on a [[Material]] by setting Details panel > Material > Blend Mode to Translucent.
Prefer to set Details pane > Material > Shading Model to Unlit, for performance.
Otherwise, if you need lit translucency set Shading Model to Default Lit.

# Details Panel Settings
Under Details panel > Translucency

## Screne Space Reflections`

Adds accurate reflections to the object.
Looks good but computationally expensive.

## Lighting Mode
Affects both performance and render quality.

- Volumetric NonDirectional
  The cheapest one.
- Volumetrix PerVertex NonDirectional
    Similar to Volumetric NonDirectional.
- Surface Forward Shading
  Most computationally costly but best looking.
  
  