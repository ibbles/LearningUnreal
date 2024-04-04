Execution nodes are nodes in a [[Blueprint Visual Script]] that have input and/or output execution pins.
Execution happens along a chain of execution nodes.
Execution nodes can have value input pins.
A value input pin may be connected to an [[Expression Node]].

Execution nodes can be disabled, meaning execution will jump over them.
Enable [[Editor Preferences]] > General > Blueprint Editor Settings > Experimental > Advanced > Allow Explicit Impure Node Disabling.
Then you will see Disable (Do Not Compile) and Enable Compile in the right-click context menu of execution nodes.
Can also set up a keyboard shortcut at [[Editor Hotkeys]] in the [[Editor Preferences]].


# References

- [_Blueprint Debugging > Enabling And Disabling Nodes_ by Epic Games, Mathew Wadstein @ dev.epicgames.com 2023](https://dev.epicgames.com/community/learning/courses/VdA/unreal-engine-blueprint-debugging/p0Gx/unreal-engine-enabling-and-disabling-nodes)
