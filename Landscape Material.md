#Landscape
#Material

A Landscape Material is a [[Material]] intended for use on a Landscape.
In particular, a Landscape Material is a Material that contains one or more Landscape Layer Blend nodes.

Landscape Materials are created just like a regular [[Material]].
Content Browser > Right-click > Material.

A Landscape Material can use Texture Sample nodes.
A Landscape Material can include [[Landscape Foliage]].
A Landscape Material can access the layer weights to produce a [[Landscape Blend Layers Material]].
A Landscape Material can get the height of the Landscape at the current point with the Absolute World Position node and a Component Mask node only returning the B (Z) component.
A Landscape Material can write to a [[Runtime Virtual Texture]].

# Texture coordinates
Tiling of textures is controlled with a Landscape Layer Coords node.
Increase `Mapping Scale` to make the texture bigger. (I think)

[UE4: Step-by-Step to Your First Landscape Material for Beginners (Day 2/3: 3-Day Tutorial Series) @ youtube.com by WorldofLevelDesign](https://www.youtube.com/watch?v=cWOlIvq0Etg)

