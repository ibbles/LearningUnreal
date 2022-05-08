# Boolean

Red.
Either true or false.
Input to Branch and other conditionals.


# Integer

Dark green blue-ish.
Whole numbers in the range -2 billion to 2 billion.


# Float

Light green.
Number with a decimal point.


# Vector

Yellow.
List of three numbers, often representing X, Y, and Z coordinates.


# Object Reference

A reference to an [[UObject]], such as a [[Blueprint Class]].
A [[Blueprint Variable|Blueprint Variable]] can limit the Object Reference to a more specific subclass of [[UObject]].


# Class Reference

A reference to a class instead of an object.
That is, a reference to a type and not an instance.
Can be used to create new instances of that type.
References to a parent class can hold a child class type.
Set this variable type by first finding the type you want a reference to and then select Class Reference instead of Object Reference.
Class References can be in an array.


# Wildcard

A variable that can have any type.
There isn't a whole lot one can do with a Wildcard, but can can pass them to a Select node.


# Array

An array of one of the non-container types.
All elements are of the same type.
Array is a container type.
To change  a variable to be an Array, click the button to the right ofthe Variable Type drop-down and select the grid of squares.
Arrays are zero-indexed, meaning that valid indices are in the range [0..n-1] for a size-n array.
