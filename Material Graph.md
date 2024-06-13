The material graph is where we define the behavior of a [[Material]] or [[Material Function]].
It is part of the [[Material Editor]].
It holds the expression network, which is a collection of connected [[Expression Node|Expression Nodes]] ending in the [[Material Output Node]].

As the number of nodes in the graph grows and their connections become increasing complex it becomes difficult to understand the intent and workings of the [[Material]].
In this case in can be helpful to split a part of the graph out into a [[Material Function]].
This allows us to replace many nodes with a single named node.

We can also use the Hide Unrelated button in the tool bar to gray out all nodes and wires that does not connect to the currently selected node.
Zoom out and you get an overview of which parts of the network is related to the selected node.

Right-click a node and select Select Downstream Nodes or Select Upstream Nodes to select all nodes they are influenced by or influences the selected node.

Nodes that are not connected to the [[Material Output Node]] can be removed by clicking the Clean Up button in the tool bar.
Beware that this will remove any nodes that are there because they are occasionally useful but not currently connected.

Nodes can be aligned with the keyboard shortcuts
- Shift + W: Top
- Shift + A: Left
- Shift + S: Down
- Shift + D: Right

You can get to the documentation for a node with right-click > View Documentation.


# Copy Paste As Text

The Material Graph nodes can be copied and pasted into and text box, such as a text editor, an email, of a chat program.
The text can then be copied, possibly on another machine, and pasted into another Material Graph.
Not entirely supported between different engine versions.
Works for other asset types as well, such as [[Blueprint Visual Script]].


# References

- [_An In-Depth Look at Environment Artist Based Tools_ > _Advanced Master Materials_ by Epic Online Learning 2021 UE4.27](https://dev.epicgames.com/community/learning/courses/3G/unreal-engine-an-in-depth-look-at-environment-artist-based-tools/BWy/unreal-engine-advanced-master-materials)
