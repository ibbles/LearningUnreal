Used to communicate between [[Blueprint Class|Blueprint Classes]].
Useful when multiple [[Blueprint Class|Blueprint Classes]] perform similar actions.

A Blueprint Interface contains **signatures** for [[Blueprint Function|Blueprint Functions]].
It does not contain an implementation for any of the functions.
The implementation is instead provided by [[Blueprint Class|Blueprint Classes]] that implement the interface.


# Creating a Blueprint Interface

**Create** a new Blueprint Interface with Content Browser > right-click > Blueprints > Blueprint Interface.
Blueprint Interface names are often prefixed with `BPI_`.
The Blueprint Interface editor contains a My Blueprint panel with a Functions category.
Click the ⊕ to add a function to the Blueprint Interface.
There is no implementation of the function in the Blueprint Interface, only its name, inputs, and outputs.
A Blueprint Interface cannot contain anything else.


# Implementing a Blueprint Interface

To add an interface to [[Blueprint Class]], do [[Blueprint Actor Editor]] > Tool Bar > Class Settings > Details panel > Interfaces > Implemented Interfaces > Add.
Functions contained in the added Interfaces are listed in the [[Blueprint Class|Blueprint Class']] My Blueprint panel, under Interfaces.
(
What if I have added an Blueprint Interface to a [[Blueprint Class]] but the Interfaces category doesn't show up in the My Blueprint panel?
What do I do then?
What did I do wrong?
One can right-click to add Blueprint Interface function event implementations,
but doing that I got a warning about name collision and the added event get a counter at the end of the name.
Some time later the Interfaces category showed up, no idea what caused it to.
Pretty sure I did recompile the [[Blueprint Class]] and that wasn't enough.
Mabe closed and opened the [[Blueprint Editor]].
This was with Unreal Engine 5.3, I think.
)
To implement an interface function, right-click the interface function in My Blueprint > Interfaces and select Implement Function.
This will create a [[Blueprint Event]] node in the Blueprint's [[Event Graph]].
The event node will have a map+arrow+gear icon.
The new event node will be executed then the interface function is called on an instance of this [[Blueprint Class]].
That is, when the corresponding message is received at the instance.

# Referencing Objects Implementing A Blueprint Interface

This doesn't seem possible.
When creating a new [[Blueprint Variable]] we can set the type to our Blueprint Interface, but I don't think that does what I expect it to.
The widget in the Details panel looks more like an [[Asset]] reference than an [[Actor]] reference.


# Calling functions

Given a reference to an [[Actor]] instance we can check if it implements a particular Blueprint Interface with Does Implement Interface.
(
Can an [[Actor Component]] also implement an interface?
)
Drag of the [[Actor]] reference output pin and search for the name of the function you want to call.
Interface functions will have a `(Message)` suffix in the list and the node has an open envelope icon.
When the Actor receives the message the [[Blueprint Event]] that implements that Blueprint Interface function in that Actor's [[Blueprint Class]] will be executed.
Messages can be sent to any [[Actor]], even those that don't implement the corresponding interface.
Such messages are ignored by the receiving [[Actor]].
Because of this the one sending the message doesn't need to know the exact type of the thing that is receiving the message.

A Blueprint Interface function can have return values.
I do not know what is returned if the thing we call the function on doesn't implement that interface.
We still get a return value from the Message node so something is being done.
Are they default constructed?
Are they safe to use?
How do we know if they are safe to use?
Does the Message node never return?

The reference can, for example be a public [[Blueprint Variable]] that a [[Level Designer]] can set on an instance of the sender, to a particular receiver instance.


# References
- [_I Struggled With Blueprint Interfaces for Years!! (Unreal Engine 5)_ by Glass Hand Studios @ youtube.com](https://www.youtube.com/watch?v=m9416Fi-PJw)
- [_Blueprint Communication > Blueprint Interfaces_ by Epic Games @ dev.epicgames.com 2023](https://dev.epicgames.com/community/learning/courses/LWv/unreal-engine-blueprint-communication/J61E/unreal-engine-blueprint-interfaces)
- [_Blueprints and Gameplay for Game Designers_ > _Opening Using a Blueprint Interface_ by Epic Games @ dev.epicgames.com 2021 UE 4.25](https://dev.epicgames.com/community/learning/courses/OP/unreal-engine-blueprints-and-gameplay-for-game-designers/0G6/unreal-engine-opening-using-a-blueprint-interface)
