Run a [[Behavior Tree]] with the Run Behavior Tree function.
A [[Behavior Tree]] is an [[Asset]] that describe a tree of tasks that the [[Pawn]] should perform.

Create a new Behavior Tree with Content Browser > right-click > Artificial Intelligence > Behavior Tree.

A Behavior Tree contains a tree of nodes.
At the top of the tree is a root node.
Types of nodes:
- Selector
- Sequence
- Task

Create new nodes in the tree by click-and-drag off of the top or bottom of an existing node.


# Task

A [[AI Task]] is a [[Blueprint Class]]

# Blackboard

The Behavior Tree has a [[Blackboard]].
This is a place where the Behavior Tree can store data.
Switch to the [[Blackboard]] view with the Blackboard button in the top-right corner.
A [[Blackboard]] is a place where a Behavior Tree can store data.
It contains a list of Keys, which are kind of like [[Blueprint Variable]]s.
Create a new Key with the New Key button in the top-left corner.


# Provided Tasks

## Move To

Uses a [[Blackboard]] to know where to move.

