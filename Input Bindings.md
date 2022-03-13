Input bindings are used to bind [[Input Events]] the player can trigger, such as pushing buttons and moving the mouse, to logic in the game.
It is an abstraction, so that the implementing logic doesn't need to specify low-level input events such as "A was pressed" och "The third gamepad axis moved up", but instead bind to named events such as "Move Left" and "Look Up".

Input Bindings are created in [[Project Settings]] > Engine > Input > Bindings.

Action Mappings binds low-level keyboard key or mouse button events to [[Action Events]].
Add a new Axis Mapping by clicking the `+` next to the Axis Mappings category header.

Axis Mappings bind low-level gamepad or mouse axis events to [[Axis Events]].
Add a new Axis Mapping by clicking the `+` next to the Axis Mappings category header.
Can also bind a key or button by hard-coding an axis value to be sent when the key is pressed or held down.
Can have multiple keys with different axis values, for example for forwards or backwards motion or with a modifier to increase or decrease the magnitude of the axis value.

The logic to execute in response to an [[Action Events|Action Event]] or [[Axis Events|Axis Event]]  may take the form of a [[Blueprint Visual Script]] node graph rooted at a [[Blueprint Events|Blueprint Event]] node or C++ code, created by right-click in the Blueprint's [[Event Graph]] and finding the event in the list.