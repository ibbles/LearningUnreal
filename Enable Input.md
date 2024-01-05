Normally all [[Input Event]] are sent to the currently possessed [[Pawn]].
Other Actors may have [[Input Event|Input Event]] nodes in their [[Event Graph]], but they will not be considered when an event happens.
We can add an Actor to the list of Actors to consider for incoming input events by classing Enable Input on it.
We must pass in the [[Player Controller]] for which we want to forward input events from.
We can get a [[Player Controller]] with the Get Player Controller function.
When an input event is received the engine will first check the Actors for which Enable Input is set and trigger the event node for the first Actor with a match.
Only after all Enable Input Actors has been checked, and none triggered, will the currently possessed [[Pawn]] be considered.

Input for a non-possessed Actor can be disabled with Disable Input.

[[Input Event]]
[[Input Bindings]]
