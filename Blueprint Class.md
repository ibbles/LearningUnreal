#Blueprint 

A Blueprint Class is a class, in the object oriented programming sense, implemented in a Blueprint.
A Blueprint class can contain [[Blueprint Variables]] and logic in the form of [[Blueprint Visual Script]].
A Blueprint class has a parent class that is either another Blueprint Class or a C++ [[UClass]].
Common parent classes for Blueprint Classes include Actor, Pawn, Character, Actor Component, Widget, ...

The intention is that common functionality can be placed in the parent classes, and more specific behavior in child classes.

# Creating A New Blueprint Class

Create a new Blueprint class by right-click in the Content Browser and select Blueprints > Blueprint Class, or Blueprint Class from the Create Basic Asset group.
Pick a parent class from the menu that appears.

An alternative way is to find the parent Blueprint Class in the Content Browser, right-click that and select Create Child Blueprint Class.

An alternative way is to find a Blueprint Class that is a child of the wanted Parent Class and duplicate that.
