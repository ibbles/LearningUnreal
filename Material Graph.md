The material graph is where we define the behavior of a [[Material]] or [[Material Function]].
It is part of the [[Material Editor]].
It holds the expression network, which is a collection of connected [[Expression Node|Expression Nodes]] ending in the [[Material Output Node]].

As the number of nodes in the graph grows and their connections become increasing complex it becomes difficult to understand the intent and workings of the [[Material]].
In this case in can be helpful to split a part of the graph out into a [[Material Function]].
This allows us to replace many nodes with a single named node.

# Copy Paste As Text

The Material Graph nodes can be copied and pasted into and text box, such as a text editor, an email, of a chat program.
The text can then be copied, possibly on another machine, and pasted into another Material Graph.
Not entirely supported between different engine versions.
Works for other asset types as well, such as [[Blueprint Visual Script]].