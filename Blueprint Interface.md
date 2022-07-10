Used to communicate between [[Blueprint Class|Blueprint Classes]].
Useful when multiple [[Blueprint Class|Blueprint Classes]] perform similar actions.

A Blueprint Interface contains **signatures for [[Blueprint Function|Blueprint Functions]]**.
It does not contain an implementation for any of the functions.
The implementation is instead provided by [[Blueprint Class|Blueprint Classes]] that implement the interface.

# Creating a Blueprint Interface
**Create** a new Blueprint Interface by Content Browser > right-click >  Blueprints > Blueprint Interface.
Blueprint Interface names are often prefixed with `BPI_`.
The Blueprint Interface editor contains a My Blueprint panel with a Functions category.
Click the ⊕ to add a function to the Blueprint Interface.
There is no implementation of the function in the Blueprint Interface, only its name, inputs, and outputs.
A Blueprint Interface cannot contain anything else.

# Implementing a Blueprint Interface
To add an interface to [[Blueprint Class]], do [[Blueprint Editor]] > Tool Bar > Class Settings > Interfaces > Add.
Functions contained in the added Interfaces are listed in the [[Blueprint Class|Blueprint Class']] My Blueprint panel, under Interfaces.
To implement an interface function, right-click the interface function in My Blueprint > Interfaces and select Implement Function.
This will create a [[Blueprint Events|Blueprint Event]] node in the Blueprint's [[Event Graph]].
The Event node will have a map/arrow/gear icon.
The new event node will be executed then the interface function is called on an instance of this [[Blueprint Class]].
That is, when the corresponding message is received at the instance.


# Calling functions
Given a reference to an [[Actor]] instance we can check if it implements a particular Blueprint Interface with Does Implement Interface.
Drag of the [[Actor]] reference output pin and search for the name of the function you want to call.
Interface functions will have a `(Message)` suffix in the list and the node has an open envelope icon.
When the Actor receives the message the [[Blueprint Events|Blueprint Event]] that implements that Blueprint Interface function in that Actor's [[Blueprint Class]] will be executed.
Messages can be sent to any [[Actor]], even those that don't implement the corresponding interface.
Such messages are ignored by the receiving [[Actor]].
Because of this the type sending the message doesn't need to know the exact type of the thing receiving the message.
We're type agnostic, just like C `void*`. Completely type-unsafe.

The reference can, for example be a public [[Blueprint Variable]] that a [[Level Designer]] can set on an instance of the sender, to a particular receiver instance.


# References
- [_I Struggled With Blueprint Interfaces for Years!! (Unreal Engine 5)_ by Glass Hand Studios @ youtube.com](https://www.youtube.com/watch?v=m9416Fi-PJw)

