Blueprint classes sometimes behave in unexpected ways for C++ programmers.
Cases where creating a Blueprint class populated with Components behave differently than an Empty Actor created in a level populated with the same Components does.
Some of these maybe bugs, sometimes difficult to tell what is a bug and what is by design.


# Blueprint Reconstruction
When editing a [[Property]] on a normal [[Actor]] created in a Level, or a [[Component]] owned by that [[Actor]], then the Property is changed on that object and a few callbacks are called and that's it.
When editing a [[Property]] on a Blueprint instance then the [[Property]] is first changed, then the entire [[Actor]] is serialized, all the [[Component|Components]] destroyed, created again, and the all the data restored from the serialization.
See [[Blueprint Reconstruction]] for more details.


# Multi-property select, single-property edit
There is a C++ function named `UObject::PostEditChangeChainProperty`.
Whenever a Property is changed in the Unreal Editor, for example from a [[Details Panel]], this function should be called on the object that was edited.
This doesn't happen when two Components in an [[Actor]] is selected and edited simultaneously if the Actor is an instance of a Blueprint, then only one of the objects is notified.

