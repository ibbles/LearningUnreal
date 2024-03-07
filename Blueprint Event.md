Events exist in the [[Event Graph]] in the [[Blueprint Actor Editor]] for a [[Blueprint Class]].
An Event node is the root of a [[Blueprint Visual Script]] execution.
Event nodes are red.
Examples of Events are Begin Play, Tick, Destroyed, End Play, Begin Overlap, End Overlap.

We can create new event nodes for existing events by right-clicking in the [[Event Graph]] and selecting the Event we want to react to.
In the [[Blueprint Actor Editor]], in the Details panel, we can also find the Events [[Category]] and click the + button next to any of the listed events, this will create a red event execution node in the [[Event Graph]].
Connect [[Execution Nodes]] to the Event node's output execution pin to build you logic.
When the Event happens the nodes connected to the Event node's output execution pin are executed.

Example events are [[Begin Play]], mouse events, keyboard events, gameplay events, and so on.

[[Custom Blueprint Event]]
[[Input Event]]
