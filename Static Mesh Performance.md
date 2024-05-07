This note discusses things to consider and settings to tweak related to rendering performance of [[Static Mesh|Static Meshes]].
See also
- [[Rendering Performance Profiling And Analysis]]
- [[Foliage Performance]]
- [[Material Performance]]
- [[Landscape Performance]]
- [[Lighting And Shadow Performance]]


# LOD

Set you [[Mesh LOD]] settings as tightly are possible, i.e. switch to a higher LOD level, i.e. fewer triangles, as close to the camera as possible.
Or use [[Nanite]] meshes, which makes this whole discussion moot.
The LOD transition sizes are set in the [[Static Mesh Editor]].


# Distance Culling

Reduce the number of meshes being drawn by setting a Max Draw Distance.
Either on each [[Static Mesh Component]], on the [[Static Mesh Asset]], or using a Cull Distance Volume.
Max Draw Distance is available on may types of [[Actor Component|Actor Components]].

# Detail Mode

Control for which scalability modes that this asset or instance will appear.
