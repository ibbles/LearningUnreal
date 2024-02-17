This note is about type introspection, for the rendering effect see [[Reflections]].

The ability of a program to examine itself.
List functions and variables.
Reference functions and variables by name.
Potentially modify itself.

C++ does not have reflection built-in.
[[Unreal Header Tool]] generates reflection information and supporting C++ code for classes marked with the `UCLASS` macro.
The generated code exposes the contents of the class to the reflection system.
Used by Unreal Editor to display and let the user modify instances of those types in a [[Details Panel]],
and to create [[Blueprint Visual Script]] nodes.


# References

- [The Unreal Build System Explained | Inside Unreal at 11:20 by Unreal Engine @ youtube.com](https://youtu.be/GJZUV8homoo?t=672)

