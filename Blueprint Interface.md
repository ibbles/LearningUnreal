Useful when multiple [[Blueprint Class|Blueprint Classes]] perform similar actions.
Used to communicate between [[Blueprint Class|Blueprint Classes]].

A Blueprint Interface contains **signatures for functions**.
It does not contain an implementation for any of the functions.
The implementation is instead provided by [[Blueprint Class|Blueprint Classes]] that implement the interface.

**Create** a new Blueprint Interface by right-clicking in the Content Browser,  Blueprints > Blueprint Interface.
Blueprint Interface names are often prefixed with `BPI_`.

# On a Blueprint Class
To add an interface to [[Blueprint Class]], do [[Blueprint Editor]] > Tool Bar > Class Settings > Interfaces > Add.
Functions contained in the added Interfaces are listed in the [[Blueprint Class|Blueprint Class']] My Blueprint panel, under Interfaces.
To implement an interface function, right-click the interface function in My Blueprint > Interfaces and select Implement Function.
This will create a [[Blueprint Events|Blueprint Event]] node in the Blueprint's [[Event Graph]].
The new event node will be executed then the interface function is called on an instance of this [[Blueprint Class]].

# Calling functions
Given a reference to an Actor instance we can check if it implements a particular Blueprint Interface with Does Implement Interface.
Drag of the Actor reference output pin and search for the name of the function you want to call.
Interface functions will have a `(Message)` suffix in the list.
When the Actor receives the message the [[Blueprint Events|Blueprint Event]] that implements that Blueprint Interface function in that Actor's [[Blueprint Class]] will be executed.
