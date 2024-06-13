A Procedural Foliage Volume is used to spawn a bunch of [[Static Mesh]]es in a volume.
It consists of
- [[Static Mesh Foliage]] [[Asset]] type.
- [[Procedural Foliage Spawner]] [[Actor]].
- Optional Procedural Foliage volume [[Actor]].
- Optional Procedural Blocking Volume.

As of Unreal Engine 5.0 Procedural Foliage is experimental and must be enabled in [[Editor Preferences]] > General > Experimental > Foliage > Procedural Foliage.
[[Static Mesh Foliage]] assets created before this setting is enabled cannot be used with Procedural Foliage.
[[Static Mesh Foliage]] assets created with the setting enabled get a Procedural category in the Details panel.


An alternative to Procedural Foliage volume is [[Procedural Content Generation PCG]].


# Static Mesh Foliage

The [[Static Mesh Foliage]] [[Asset]] specifies a [[Static Mesh Asset]] and contains a bunch of settings that affect how instances of this foliage type interacts with other foliage instances, both of the same type and other types.

Create with [[Content Browser]] > right-click > Foliage > Static Mesh Foliage.
The [[Static Mesh Foliage]] is used by adding it to a Procedural Foliage Spawner's Foliage Types array.
There are often multiple [[Static Mesh Foliage]] types used per Procedural Foliage Spawner.

Some notable settings:

- Mesh: The [[Static Mesh Asset]] to spawn.
- Density Falloff: Reduction in density for instances spawned close to the bounds of the Procedural Foliage Volume.
- Procedural
	- Collision
		- Collision Radius: How close together instances of this [[Foliage Type]] can be. This can have a bit impact on the final instance density, together with Num Steps, Initial Seed Density, and Seeds Per Step.
		- Shade Radius: The size of the area around each instance that is in shade.
			- There is another setting controlling whether or not a [[Foliage Type]] can grow in shade.
- Growth
	- Can Grow In Shade: If not enabled then no instances of this [[Foliage Type]] will be created in the shade of another instance of any [[Foliage]]. Shade settings are set in the Collision category.
	- Max Age: How log an individual will remain alive during the simulation. Older individuals have a larger scale than younger individuals, based on Procedural Scale.
	- Procedural Scale and Scale Curve: How an individual changes size as it grows. The Scale Curve can be modified. Right-click a control node and select Auto to take control over the tangent direction.


# Procedural Foliage Spawner

The Procedural Foliage Spawner is an [[Asset]] that has a list of [[Static Mesh Foliage]] [[Asset]] references.
It will spawn instances of the [[Static Mesh Asset]] described by those [[Static Mesh Foliage]]s.
Create with [[Content Browser]] > right-click > Foliage > Procedural Foliage Spawner.
Procedural Foliage Spawner assets often has the `FS_` prefix.
Contains a few parameters used by the Procedural Foliage Volume.
And a Foliage Types array, which should contain the [[Static Mesh Foliage]] types that should be spawned.

Settings:
- Procedural Foliage
	- Allow {Landscape, Static Mesh, Translucent, Foliage}: The types of objects that instance can be spawned on.

When dragged into the [[Level]] the Procedural Foliage Spawner creates a rectangular volume in which instances are spawned.
It acts like a Procedural Foliage Volume with itself as its Foliage Spawner.
Place and scale the Procedural Foliage Spawner to cover the area where you want instances to spawn.
Click [[Details Panel]] > Procedural Foliage > Resimulate to


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