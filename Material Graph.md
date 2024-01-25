The material graph is where we define the behavior of our [[Material]]
It is part of the [[Material Editor]].
It holds the expression network, which is a collection of connected [[Expression Node|Expression Nodes]] ending in the [[Material Output Node]].

As the number of nodes in the graph grows and their connections become increasing complex it becomes difficult to understand the intent and workings of the [[Material]].
In this case in can be helpful to split a part of the graph out into a [[Material Function]].
This allows us to replace many nodes with a single named node.
