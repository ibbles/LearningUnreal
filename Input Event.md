An Input Event is an event that the player can trigger.
Input events are bound to one or more low-level events through [[Input Bindings]].
There are two types of input events: [[Action Event]] and [[Axis Event]].

When the players presses a button the bindings dictate which Input Event should be triggered, but not which [[Blueprint Event]] that should be executed.
There may be many Actors listening for the same event.
These Actors are sorted based on priority and only the first in this list that has a [[Blueprint Event]] for the Input Event get to see it.
First in the list are Actors we've explicitly called [[Enable Input]] on.
Last in the list is the current Player Controller.
(
Is the possessed Pawn in the list? I assume so.
)


If an [[Axis Event]] is not getting the axis value you expect, and instead always zero, it is possible that the [[Input Mode]] has been set to UI Only on the [[Player Controller]].
This is often done when displaying a menu or other overlay UI element.
To resume control over the [[Pawn]] remember to change the [[Input Mode]] back to Game Only.