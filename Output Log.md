The Output Log panel is where we can see messages, warnings, and errors printed by either Unreal Engine, Unreal Editor, or the project.
Messages are white, warnings are orange, and errors are read.
Use the Filters drop-down menu to select which outputs to display.

When printing a string or text from a [[Blueprint Visual Script]] prepend either `warning: ` or `error: ` to make the printed text show up in either orange or red, respectively.
This only works for the Output Log panel, not the terminal.


# Logging From Blueprint Visual Script

Print Text or Print String.


# Logging From C++

```cpp
UE_LOG(LogTemp, Warning, TEXT("Some data: %d."), MyInt);
```

TODO Write about adding a project-specific log category to replace `LogTemp`.


# Logging In Shipping Builds

Unreal Engine has a flag for enabling logging in shipping builds.
Add `bUseLoggingInShipping = true; ` to you `MYPROJECT.Target.cs` files.
This does not work.
The project cannot be compiled with this flag enabled.

One suggestion for making it work is to turn on unique builds.
This gives an error I don't remember anymore.

One suggestion for making it work is to turn on `bOverrideBuildEnvironment`.
This leads to linker errors on `UE::Logging::Private::BasicLog`.

I think one must have a source build of Unreal Engine for this to work,
which I don't have.
