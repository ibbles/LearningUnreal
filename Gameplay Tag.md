Gameplay Tags are hierarchical boolean flags.
A particular tag is either enabled or disabled.
Nesting is done with `.`: `RootTag.BranchTag.LeafTag`.
At runtime they are stored in variables of type [[Gameplay Tag Container]].
At edit time they are stored in `.ini` files.
Multiple [[Gameplay Tag Container|Gameplay Tag Containers]]  can use the same `.ini` file,
making it possible to share tag sets between classes.

The  hierarchical nature of Gameplay Tags means that you can query for a parent tag and get back true if any child tag of that parent tag is enabled.

An example use-case is status effects:
- `Status`
	- `Buff`
		- `Fast`
		- `Strong`
		- `Focused`
	- `Debuff`
		- `Slow`
		- `Weak`
		- `Dazed`
	- `Healing`
		- `Nature`
		- `Divine`
		- `HealthPack`
	- `Damage`
		- `Heat`
			- `Fire`
			- `Magma`
			- `Radiation`
		- `Cold`
			- `Ice`
		- `Corrosion`
