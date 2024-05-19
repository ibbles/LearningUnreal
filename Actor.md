An Actor is the base class for everything that can exists within a [[Level]].
Not just graphical things, but also abstract thing such as [[Volumes|Volume]] or [[Controller|Controllers]].
Example Actor types:
- [[Sound]]
- [[Niagara System]]
- [[Reflections|Reflection Capture]]
- [[Light Source]]
- [[Trigger Volume]]
- [[Post Process Volume]]
- [[Blueprint Actor]]
- and many more

An Actor can be added to the level using [[Main Tool Bar]] > Place Actor > drag an Actor type into the [[Level Viewport]] where you want it.
The Place Actor menu is searchable, meaning you can type the name of the Actor type you want to filter the list.
Each Actor type may be part of multiple categories and may therefore appear multiple times in the list.

An Actor can be added to the level by dragging an [[Asset]] from the [[Content Browser]] onto the [[Level Viewport]].

We can **spawn** new Actor instances at runtime with the Spawn Actor From Class node.
We can **destroy** an Actor instance at runtime with the Destroy Actor node.

# Attach Actor To Actor

An Actor can be attached to another Actor.
Attach by dragging on Actor on top of another in the [[Outliner]].
The dragged Actor will be attached to the Actor it is dropped on.
When the parent Actor is moved then the child Actor will also move.
When the parent Actor is hidden, with the eye icon in the [[Outliner]], then the child Actor is hidden as well.
Detach the attached Actor again by dragging it on top of the parent Actor.
