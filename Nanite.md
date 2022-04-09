Nanite is a per-triangle [[LOD]] system for [[Static Mesh|Static Meshes]].
Instead of swapping an entire mesh between different LOD levels it groups triangles into patches and does LOD selection per patch.

Enable Nanite for a particular [[Static Mesh Asset]] by checking Static Mesh Editor > Details > Nanite Settings > Enable Nanite Support.

When importing e.g. an FBX mesh one of the options i Build Nanite.
Enable Build Nanite to turn the imported Static Mesh into a Nanite mesh.

The Nanite system comes with a bunch of in-editor visualization modes.
Enable with Viewport > Lit > Nanite Visualization.
