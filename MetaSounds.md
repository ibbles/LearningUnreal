Introduced in Unreal Engine 5.
Still a beta [[Plugin]] in Unreal Engine 5.0, must be enabled in Top Menu Bar > Edit > Plugins.

Audio system that gives complete control over audio rendering.
Drive sound in a similar way to the [[Material]] graph.
Audio shaders.

Each MetaSound is an [[Asset]] that lives in the [[Content Browser]].
Create a new MetaSound by Content Browser > right-click > Sounds > MetaSound.
To use a MetaSound in a Blueprint create an Audio Component and set Detals panel > Sound > Sound to the MetaSound asset.

# MetaSound Editor

The MetaSound Editor contains a Visual Script graph with input and output nodes.
The input nodes are also called Event nodes, since events causes them to trigger and their Execution subgraph be executed.
We define the sound by connecting the input node to the output node with Execution and Expression nodes in between.
Execution flow from Execution node to Execution node along the white event wires.
Each executed Execution node evaluate its input pins, causing all Expression node along that subgraph to be evaluated.
Execution nodes are nodes that can be executed.
Execution nodes must be part of a path connected to an Event node in order to have an effect.
Expression nodes are nodes that take input and produce output but have no side-effects.
All expression node paths must connect to an Execution node input pin in order to have an effect.
Connect a node that outputs sound to the Out pin on the Output node.
Cannot connect multiple sound generating nodes to the output node.
Multiple sound generating nodes can be merged with a Mixer node.

Multiple execution wires can we connected to the same execution input pin by passing them through a Trigger Any node.


Click Play in the tool bar to hear your sound.

## Events

Events are input nodes that trigger graph execution.
The default Input node is an event node.
Create a new Event node by  dragging from an Execution input pin and select Promote To Graph Input.

Event nodes can be triggered from within the MetaSound Editor while the sound is playing by selecting the node and clicking the Simulate button in the Inspector panel.

Event inputs should have their Type set to Trigger in the Inspector panel.


## Input

We can add an input to the MetaSound from the Members panel.
The Event nodes are examples of inputs.
We can also have data inputs, so we can pass in things such as booleans and floats.
To set the value of a data input add a Set TYPE Parameter node to the Blueprint that owns the Audio Component that is using the MetaSound asset.
To execute a trigger input add an Execute Trigger Parameter to the Blueprint that owns the Audio Component.

## Nodes

### Wave Player

A node that plays a sound file.
Will play the sound only once.
Unless the Loop input pin is set to true.

If triggered again then the current playing sound will stop and start over.
Cannot be used to overlap a sound with itself.
Must have multiple Wave Player nodes for that.

### Envelope

Produces a value that changes over time.
Is it like a [[Lerp]], but with ease-in and ease-out?

### Trigger Repeat

An Execution node that triggers its output Execution pin repeatedly with a delay.

### BPM To Seconds

Useful in combination with Trigger Repeat

### Random Get

Randomly select an element in an array.
Useful for randomly selecting a sound to play in a looping Wave Player.

### Trigger Counter

Count the number of times the node is triggered.
Can be used in conjunction with a bunch of Trigger Compare to round-robin between different execution paths.

### Send/Receive

A way to send values across the graph.
Give  a data wire a name and read it elsewhere.
Even triggers can be sent.
These are global, i.e. they can be used to communicate between MetaSounds.

# References
- [_MetaSounds: The Next Generation Sound Sources_ @ docs.unrealengine.com](https://docs.unrealengine.com/5.0/en-US/metasounds-the-next-generation-sound-sources-in-unreal-engine/)
- [_MetaSounds in UE5: From Miniguns to Music | Unreal Engine_ by Unreal Engine @ youtube.com](https://www.youtube.com/watch?v=3230-FwCts0)

