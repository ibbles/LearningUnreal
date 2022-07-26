As of Unreal Engine 5.0 Procedural Foliage is experimental and must be enabled in [[Editor Preferences]] > General > Experimental > Foliage > Procedural Foliage.


# Static Mesh Foliage

[[Static Mesh Foliage]] assets created before this setting is enabled cannot be used with Procedural Foliage.
[[Static Mesh Foliage]] assets created with the setting enabled get a Procedural category in the Details panel.

The [[Static Mesh Foliage]] contains a bunch of settings that affect how instances of this foliage type interacts with other foliage instances, both of the same type and other types.

- Density Falloff: Reduction in density for instances spawned close to the bounds of the Procedural Foliage Volume.
- Collision
	- Collision Radius: How close together instances of this [[Foliage Type]] can be.
	- Shade Radius: The size of the area around each instance that is in shade.
		- There is another setting controlling whether or not a [[Foliage Type]] can grow in shade.
- Growth
	- Can Grow In Shade: If not enabled then no instances of this [[Foliage Type]] will be created in the shade of another instance of any [[Foliage]]. Shade settings are set in the Collision category.

# Procedural Foliage Spawner

Create a [[Content Browser]] > right-click > Foliage > Procedural Foliage Spawner.
Contains a few parameters for the actual spawner.
And a Foliage Types array, which should contain the [[Static Mesh Foliage]] types that should be spawned.


# Procedural Foliage Volume

Create a Procedural Foliage Volume [[Actor]].
Set Location and Scale to cover the are in which  you want to spawn [[Foliage]].
Make sure the volume is tall enough.
Not sure what that means.
Tall enough to cover the [[Landscape]] height variations?
Tall enough to fit the spawned [[Static Mesh]] inside it?

Set Details panel > Procedural Foliage > Foliage Spawner to the Procedural Foliage Spawner [[Asset]] described above.

Click the Details panel > Procedural Foliage > Resimulate button.
If the volume covered by the Procedural Foliage Volume has changed then you may need to click the Load Unloaded Areas button next to the Resimulate button first.


# Procedural Blocking Volume

A negation of a Procedural Foliage Volume, used to create clearings and paths in the forest.


# Landscape Layers

It is possible to create inclusive and exclusive [[Landscape]] layers so that we can paint where [[Foliage]] should be spawned, much like the Grass node in [[Landscape Foliage]].

Don't know how to do this yet.


# References