Debugging is the process of finding and correcting mistakes and errors software.

# Print String And Print Text

Values and debug messages can be printed with the Print String and Print Text nodes.
The string will be printed to the top-left corner of the [[Viewport]], to the [[Output Log]], and to the terminal if Unreal Editor was opened from one.
This is useful to verify that a particular piece of code is being executed, or to see the value of some input or variable.

I prefer Print Text over Print String because for text we have the Format Text node.

Print String and Print Text have a Key input.
This makes it possible to update an already printed text instead of adding a new line ever time.
This is useful when printing a value that updates over time, especially when there are multiple values we want to print.

A printed string or text that starts with `warning: ` will show up in orange in the [[Output Log]].
A printed string or text that starts with `error: ` will show up in red in the [[Output Log]].
These colors do no affect the color of text printed to the terminal, if Unreal Editor was opened from one.
That makes Print String and Print Text different from `UE_LOG`.


# Wire Execution Highlight

With a [[Blueprint Visual Script]] open during a [[Play In Editor]] session execution wires will highlight when those nodes are executed.
This only works for one object at a time.
To select which object use the object selection drop-down in the tool bar.


# Breakpoints

Create or remove a breakpoint on a node either by right-clicking and selecting Toggle Breakpoint, or by selecting the node and pressing F9.
The breakpoint is shown as a white disk.
Breakpoints can be disabled, in which case they are shown as a white circle.
Breakpoints are associated with a particular instance of a Blueprint.
Select which instance is being debugged with the drop-down in the tool bar.

When stopped the current node is marked with a white arrow head.
This is the next node to be executed.
You can hover over output pins going into the current node to see their values.
Dependee pins can also be inspected.
Pins on other nodes can (usually?) not be inspected because they are not currently being evaluated.

When stopped on a breakpoint you can:
- F10: Step over the current node. Execution moves to the next node along the execution wire.
- F11: Step into the current node. Works for functions and macros.
- Alt+Shift+F11: Step out. Execute all nodes along the current execution wire and stop at the next node in the parent graph. Works when inside a function or macro. The inverse of step into.


# Watches

Right-click a node pin and select Watch This Value.
Then, when a Play In Editor session is open, select an instance of the Blueprint Class from the Debug Filter in the Tool Bar of the [[Blueprint Actor Editor]].
The last value that the pin had will be shown in a tool-tip.
Remove the watch by clicking the icon next to the pin.



# Blueprint Debugger Window

Opened with [[Top Menu Bar]] > Debug > Blueprint Debugger.

## Call Stack

This is a tab in the Blueprint Debugger window.
Works together with Breakpoints.
When stopped on a breakpoint the Blueprint Debugger window will display the current call stack.
Click on a function in the call stack to jump to the corresponding [[Execution Node]].

## Data Flow

This is a tab in the Blueprint Debugger window.

Select a Blueprint in the Select Blueprint drop-down.
Instances of the selected Blueprint is shown in the first table.
Expand the instances to display their data.
The display is updated as the game is running.

The bottom table show the breakpoints we have set and execution races.
I don't know what an execution trace is yet.
All breakpoints can be enabled or disabled from the button at the top of the Data Flow tab.


# References

- [_Blueprint Debugging_ by Epic Games @ dev.epicgames.com 2023](https://dev.epicgames.com/community/learning/courses/VdA/unreal-engine-blueprint-debugging/xnmJ/unreal-engine-blueprint-debugging-overview)
