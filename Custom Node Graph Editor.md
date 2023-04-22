See also [[Custom Node Graph Editor References]] for a summary of other sources on custom node graph editors.

A node graph editor is an editor used to edit assets that contain graph data.
Examples of built-in graph editors are the [[Blueprint Visual Script Editor]], the [[Material Editor]], and the [[Sound Cue Editor]].
For a description of how the [[Sound Cue Editor]] has been built, see [[Sound Cue Node Graph Editor]].

This note describe how to build a custom node graph editor for a new project-specific asset type.
The example asset is a transportation network.
It consists of facilities and transportation channels.
A facility can have zero or more inputs and zero or more outputs.
Each facility input and output is of a particular kind.
A facility output can be connected to facility input via a channel.
A facility output may only be connected to a facility input of the same kind.

Example facilities are
- Factory.
	- No inputs.
	- Outputs items on a conveyor.
- Packager.
	- Inputs items on a conveyor.
	- Outputs packages in a box truck.
- Warehouse.
	- Inputs packages in a box truck.
	- Outputs packages in a box truck.
- Port.
	- Inputs packages in a box truck.
	- Outputs containers on a ship.
	- Inputs containers on a ship.
	- Outputs packages in a box truck.
- Truck stop.
	- Inputs packages in a box truck.
	- Inputs containers on a container truck.
	- Outputs containers on a container truck.
	- Outputs packages in a box truck.
- Store.
	- Inputs packages in a box truck.

The channels are
- Conveyor. Transports items.
- Box truck. Transports packages.
- Ship. Transports containers.
- Container truck. Transports containers.

Important engine classes.
These are the classes that we will either use or subclass to build our custom node graph editor.
- `UObject`
	- Base class for our Transportation Network asset.
- `UEdGraph`
- `UEdGraphNode`
- `UEdGraphPin`
- `UEdGraphEditor`

Important custom classes.
There are the classes what we will write for our custom graph node editor.

