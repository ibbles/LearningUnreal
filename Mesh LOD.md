Mesh LOD means that there are multiple versions of a [[Static Mesh]] (or [[Skeletal Mesh]]?) or varying complexity and depending on the distance or size on screen the renderer selects an appropritate version.
The purpose is to improve performance by not rendering more detail than necessary.

We can see the LOD levels available to a [[Static Mesh]] in the [[Static Mesh Editor]], in Details panel > LOD Picker > LOD.
The transition point, when to switch between LOD levels is set by selecting a LOD level with the LOD Picker and then editing Screen Size in Details panel > LOD # (the LOD level picked) > Screen Size.
Screen Size will be write-protected if Details panel > LOD Settings > Auto Compute LOD Distances is checked.
We can set Details > LOD Settings > Minimum LOD to limit how details LOD version a particular [[Static Mesh Asset]] will use.

We can force a particular [[Static Mesh Component]] to use a particular LOD level with Details panel > LOD > Forced LOD Model.

The most extreme form of LOD is to not render a mesh at all.
This can be done for a [[Static Mesh Component]] with Details panel > LOD > **Desired Max Draw Distance**.
When the mesh if further from the camera then this then it won't be rendered.
It's "desired" because the distance can be set by other things as well, such as a Cull Distance Volume (I think.).
A mesh can also be prevented from rendering by setting Details panel > LOD > **Detail Mode**.
The Detail Mode, one of Low, Medium, High, control at what details settings this mesh should start to be included in.
If detail mode >= system detail mode, then the mesh won't be rendered.
Set small clutter to High, meaning that they are rendered only on High settings, set the important bits to Low, meaning that they are rendered on Low and up.
```
    m e s h
    l  m  h
s l ✓  x  x
y m ✓  ✓  x
s h ✓  ✓  ✓
```

