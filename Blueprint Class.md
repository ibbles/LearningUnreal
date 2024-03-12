A Blueprint class is a class, in the object oriented programming sense, implemented in a Blueprint.
A Blueprint class can contain [[Blueprint Variable]] and logic in the form of [[Blueprint Visual Script]].
A Blueprint class has a parent class that is either another Blueprint Class or a C++ [[UClass]].
Common parent classes for Blueprint classes include [[Actor]], [[Pawn]], [[Character]], [[Actor Component]], [[Widget]], [[Game Mode]], [[Game State]], and more.

The intention is that common functionality can be placed in the parent classes, and more specific behavior in child classes.

# Creating A New Blueprint Class

Create a new Blueprint class by right-click in the Content Browser and select Blueprints > Blueprint Class, or Blueprint Class from the Create Basic Asset group.
Pick a parent class from the menu that appears.

An alternative way is to find the parent Blueprint class in the Content Browser, right-click that and select Create Child Blueprint Class.

An alternative way is to find a Blueprint class that is a child of the wanted Parent Class and duplicate that.

Blueprint classes often have names that start with `BP_`.

# Child Class

A Blueprint class can have child classes.
This is called inheritance.
A child class inherits variables and functions from the parent class.
Can add additional variables and function.
A child class can provide a new implementation for functions in the parent class. (I assume.)
This provides specialization.

An object reference typed to the parent class can point to an instance of a child class.
The opposite is not true, except for the parent class instances that are of the child class type.
We can cast a parent type reference to a child type reference and will get None/Null/Invalid if the instance isn't of the child class.

Inheritance can be many levels deep.
Many Blueprint classes eventually inherit from one of the built-in Unreal Engine classes, such as [[Actor]] or [[Component]].

Create a new child class of a Blueprint class by Content Browser > right-click the Blueprint class > Create Child Blueprint Class.
The parent of a Blueprint class is shown in the top-right corner of the [[Blueprint Actor Editor]].
The parent class of a Blueprint class can be changed from [[Blueprint Class Editor]] > [[Class Settings]] > Parent Class.


# Class Reference Variable
A class reference variable is a [[Blueprint Variable]] that points to a class type instead of an instance of a class.
