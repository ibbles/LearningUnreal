#Blueprint 

Events exist in the [[Event Graph]] in the [[Blueprint Editor]] for a [[Blueprint Class]].
An Event node is the root of a [[Blueprint Visual Script]] execution.
Event nodes are red.
Examples of Events are Begin Play, Tick, Destroyed, End Play, Begin Overlap, End Overlap.

We can create new event nodes for existing events by right-clicking in the [[Event Graph]] and selecting the Event we want to react to.
Connect [[Execution Nodes]] to the Event node's output execution pin to build you logic.
When the Event happens the nodes connected to the Event node's output execution pin are executed.

Example events are [[Begin Play]], mouse events, keyboard events, gameplay events, and so on.

[[Custom Blueprint Events]]
[[Input Events]]