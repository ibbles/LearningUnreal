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
This will create a [[Event Node]] in the Blueprint's [[Event Graph]].

