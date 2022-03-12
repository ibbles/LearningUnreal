Input bindings are used to bind [[Input Events]] the player can trigger, such as pushing buttons and moving the mouse, to logic in the game.
It is an abstraction, so that the implementing logic doesn't need to specify low-level input events such as "A was pressed" och "The third gamepad axis moved up", but instead bind to named events such as "Move Left" and "Look Up".

Input Bindings are created in [[Project Settings]] > Engine > Input > Bindings.

The logic to execute in response to an input may take the form of a [[Blueprint Visual Script]] rooted at a [[Blueprint Events|Blueprint Event]] or C++ code.