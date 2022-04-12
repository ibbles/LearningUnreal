An Input Event is an event that the player can trigger.
Input events are bound to one or more low-level events through [[Input Bindings]].
There are two types of input events: [[Action Events]] and [[Axis Events]].

When the players presses a button the bindings dictate which Input Event should be triggered, but not which [[Blueprint Event]] that should be executed.
There may be many Actors listening for the same event.
These Actors are sorted based on priority and only the first in this list that has a [[Blueprint Event]] for the Input Event get to see it.
First in the list are Actors we've explicitly called [[Enable Input]] on.
Last in the list is the current Player Controller.
(
Is the possessed Pawn in the list? I assume so.
)
