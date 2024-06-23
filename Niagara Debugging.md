# Attribute Spreadsheet

Opened from [[Niagara Editor]] > Top Tool Bar > Debug > Attribute Spreadsheet.
The Attribute Spreadsheet panel opens.
Click the Capture button at the top to capture the current state of the particle system and display all per-particle data in the Attribute Spreadsheet panel.
The preview simulation is paused so that we can see the rendered state associated with the captured particle data.


# Debug Drawing

Some [[Niagara Module]]s have debug drawing functionality.
This is shown as an eye button on the module in the Emitter's module stack.
Click the eye button to enable or disable debug drawing for that module.
The drawn shapes show up in the preview [[Viewport]].

Add debug drawing to your own [[Niagara Module]] by reading the LOCAL Debug Draw variable with a Map Get node.
This is an object that provides a bunch of Draw SHAPE functions.
Use any of the Draw SHAPE functions.
I don't know if we will get one debug shape per particle, or if debug drawing is filtered in some way automatically.

To support toggling, add a Static Switch with Static Switch Type set to Enum Constant and Compiler Constant set to Function Debug State.
Have one execution wire path that does the debug rendering and one that does not.
Connect the execution wire with debug rendering to the If Basic input pin,
and the execution wire without rendering to the If No Debug input pin.
Put a Map Set node before and after the debug rendering part of the graph to avoid having to duplicate any logic nodes that should always be executed.


# Niagara Debugger

This is used to debug [[Niagara System]] instances with in the [[Level]].
Click [[Details Panel]] > Niagara Utilities > Debug to open the Niagara Debugger.
Contains a number of parameters that can be set.

Has a Performance tab.

# References

- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
