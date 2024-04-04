Input bindings are deprecated since Unreal Engine 5.1.
Use [[Enhanced Input]] instead.

Input bindings are used to bind [[Input Event]] the player can trigger, such as pushing buttons and moving the mouse, to logic in the game.
It is an abstraction, so that the implementing logic doesn't need to specify low-level input events such as "A was pressed" or "The third gamepad axis moved up", but instead bind to named events such as "Move Left" and "Look Up".

Input Bindings are created in [[Project Settings]] > Engine > Input > Bindings.


# Action Mappings

Action Mappings binds low-level **keyboard key** and **mouse button** events to [[Action Event]]s.
Actions are binary events, they are either not active or fully active.
An [[Actor]] that listens for an [[Action Event]] has a [[Blueprint Event]] node in its [[Event Graph]].
The [[Blueprint Event]] node for an [[Action Event]] has a Pressed execution wire and a Released execution wire.
Add a new Axis Mapping by clicking the `+` next to the Axis Mappings category header.
Each Action Mapping has a **name**, a **key or button** that should trigger the action, a set of **modifiers** that should be held.
Chose a key or button either from the drop-down list or by clicking the Select button and type or press the key or button to create a mapping for.


# Axis Mappings

Axis Mappings bind low-level **gamepad** and **mouse axis** events to [[Axis Event]]s.
Add a new Axis Mapping by clicking the `+` next to the Axis Mappings category header.
The [[Axis Event]] node has a Value output pin providing the current value of the axis.
Can also bind a **key or button** by hard-coding an axis value to be sent when the key is pressed or held down.
Can have multiple keys with different axis values, for example for forwards or backwards motion or with a modifier to increase or decrease the magnitude of the axis value.


# Acting On Input

The logic to execute in response to an [[Action Event]] or [[Axis Event]]  may take the form of a [[Blueprint Visual Script]] node graph rooted at a [[Blueprint Event]] node or C++ code.

A [[Blueprint Event]] node created by right-click in the Blueprint's [[Event Graph]] and finding the name of the action or axis mapping in the list.
They will be under Input > {Action, Axis} Events.
This will add a root execution node to the graph.
For a [[Pawn]] it is common to simply forward the Event outputs to the inputs of a Add Movement Input or Add Controller {Yaw, Pitch, Roll} Input node.

Many Actors may be listening for [[Input Event]] but only one will receive it.
The [[Player Controller]] is always in the list of candidates, but so is any other Actor that has had the [[Enable Input]] function called on it.
