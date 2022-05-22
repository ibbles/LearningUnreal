Unreal Engine uses custom build pipeline.
It is a compiler driver, just like Make or Ninja.

Consists of two parts:
- Unreal Build Tool
- Unreal Header Tool

Unreal Build Tool runs Unreal Header Tool when necessary.

Unreal Build System can generate project files for various IDEs.
To make working with the code base easier.
The build system ignore the project files, they are for the IDE only.
Hitting Compile in the IDE will run the Unreal Build System, not whatever build system the IDE uses.

# Build cycle

Hit compile > Scan modules > Ignore unchanged > Detect Unreal Macros > Code generation > Call compiler > Macro expansion > Compilation > Linking


# Unreal Header Tool and Unreal Macros

The main purpose of Unreal Header Tool is to parse Unreal Macros in C++ header files and produce generated C++ code that implement what those macros describe.
Produces `<FILE>.generated.h`, where `<FILE>` is the base name of the parsed header file.
The generated header file should be included after all other includes in the parsed header file.

Example macros: [[USTRUCT]], [[UCLASS]], [[GENERATED_BODY]], [[UFUNCTION]], [[UPROPERTY]].
Unrel Build Tool invokes Unreal Header Tool when it detects those macros in a header file.
Unreal Header Tool generates C++ code to implement what was described by the macros.
Used to support things like [[Reflection]], serialization, Blueprint integration, Gameplay Abilities/Effects system,

For some function declarations, such as those with the `Server` [[Function Specifier]], is implemented in the generated code and the user code is put in a function with the `_Implementation` suffix.

Avoid `#ifdef` macros in headers parsed by Unreal Header Tool.
Can lead to mismatch between the generated code and the preprocessed code.


# References
- [The Unreal Build System Explained | Inside Unreal by Unreal Engine @ youtube.com](https://www.youtube.com/watch?v=GJZUV8homoo)

