The Blueprint Visual Script Editor is used to create logic and behavior for our [[Blueprint Class|Blueprint Classes]].
The logic is defined by [[Execution Node]]s that are linked together via their execution pins and [[Expression Node|Expression Nodes]] that control and configure the [[Execution Node]]s for each execution, and [[Value Nodes]] that provide input to both the [[Execution Node]]s and the [[Expression Node|Expression Nodes]].

# Creating nodes
New nodes are added to the Visual Script by right-clicking and then selecting the wanted node from the list that appears.
The node list can be filtered.

References to **[[Blueprint Variable]]** can be created either from the right-click menu or by dragging the variable into the graph from the Variables category in the My Blueprint panel.
We can also create references to **Components** in a [[Blueprint Class]] by dragging the Component from the Components panel to the graph.
Upon release we get to chose if we want a get node or a set node.
By holding Ctrl while dragging we create a get node immediately.
By holding Alt while dragging we create a set node immediately.
Dragging a Component creates a set node by default, no need to hold Ctrl.
By releasing on top of a node pin we directly connect the variable to that pin.

# Connecting and disconnecting nodes
Nodes are connected at their input and output pins with **wires**.
An **input pin** can be connected to an **output pin** by dragging from one to the other.
Either direction works.
The **Target input pin** can be connected to multiple Object References, the function will be called on all of them.
A wire can be **disconnected** from a pin by right-clicking and selecting Break Link To ... or holding Alt and left-click.
Execution wires can be merged at input pins, meaning that shared functionality can be placed at the end of two wires of execution.
There cannot be multiple execution wires going out of an execution output pin.
Data pins for struct datatypes can be split to show their members by right-click and select **Split Struct Pin**.
This can be done recursively for nested structs.
The struct pins can be merged back together again with **Recombine Struct Pin**.
An alternative is a **Break STRUCT node**, which does the same but can hide unused pins.

# Organizing nodes and wires
**Reroute** nodes can be created by double-clicking on a wire.

Nodes can be moved so that the wires are **straightened** by selecting multiple nodes and pressing `Q`.
Nodes will only be moved up or down.
Often the anchor node for the straightening is the last selected node.
Not always though, not sure how to actually control the anchor node.
I often organize a network of [[Expression Node|Expression Nodes]] that goes into an [[Execution Node]] by:
- Select the Execution Node.
- Hold Ctrl while selecting the Execution Node connected to the execution input pin of the first Execution Node.
- Type `Q`. This will move the Execution Node in level with the Execution Node connected to the execution input pin.
- Click-and-drag to select all Expression Nodes.
- Hold Ctrl and click the Execution Node.
- Type `Q`. This will move the Expression Nodes to straighten out the connection wires between the selected nodes, often without moving the Execution Node.

Nodes can be **aligned** up, down, left, or right by holding Shift and typing `W`, `S`, `A`, or `D`.

**Long chains** of [[Execution Node]]s can be **split into separate chains** with a Sequence Node.
Create a Sequence Node by holding `S` and clicking on the Node Graph background.

# Zoom
We can zoom the graph in and out using the scroll wheel on the mouse.
By default the max zoom is 1:1.
By holding control we allow zooming in further than 1:1, giving us magnification.

# Comments
A collection of nodes can be surrounded by a comment.
This is a bit like a grouping in that moving the comment will move all the nodes inside it.
Comment can be colored and have different text sizes, both edited from the Details panel.
