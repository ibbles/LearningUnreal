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


# Layer Sampling

The Landscape Material can check how much of each [[Landscape Painting]] layer is present at the current fragment.
This is done with a  Landscape Layer Sample node, called Sample '<LAYER`_`NAME>' in the [[Material Graph]].
The node returns a value in 0..1 based on how much that particular layer has been painted at this particular location.



# Distance Based Quality

It is common to use a Far Blend Distance parameter to control [[Material]] complexity based on distance from the camera.
Change
- Texture Tiling.
- (Fill in more here.)

[_Virtual Texturing | Live from HQ | Inside Unreal_ - Far Blend Distance Parameter, by Unreal Engine @ youtube.com. 2019](https://youtu.be/fhoZ2qMAfa4?t=1927)

# References

- [_Landscape Communication in UE4 (Automatic Snow/Desert Foliage!)_, by PrismaticaDev @ youtube.com](https://www.youtube.com/watch?v=rW4zCzuGZvs)

