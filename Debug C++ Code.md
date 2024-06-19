Unreal Engine provide debugger-specific visualizers for Unreal Engine types.
These are stored in `$UR_ROOT/Engine/Extras/`.
For example, visualizer for GDB are stored in the `GDBPrinters` directory and visualizers for LLDB are stored in the `LLDBDataFormatters` directory.
There are others as well.
Not all of these are included with binary distributions of the engine.
If you need one that is missing then download from https://github.com/EpicGames/UnrealEngine/tree/release/Engine/Extras (requires log in and GitHub account linking with an Epic Games account.)
You should match the Git tag with the engine version you have installed.

Some visualizers require some setup to work.
The individual visualizer files in `$UE_ROOT/Engine/Extras/` typically explain what to do.
For example, from `$UE_ROOT/Engine/Extras/LLDBDataFormatters/UEDataFormatters.py`:
```python
# To install:
# 1) Open Terminal and run:
#        touch ~/.lldbinit
#        open ~/.lldbinit
# 2) Add the following text to .lldbini and save - modifying the path as appropriate:
#        settings set target.inline-breakpoint-strategy always
#        command script import "/Path/To/Epic/UE/Engine/Extras/LLDBDataFormatters/UEDataFormatters.py"
```

Rider seems to pick up the LLDB visualizers automatically.


# References

- [_Debug Unreal Engine Project_ by JetBrains @ jetbrains.com/help 2024](https://www.jetbrains.com/help/rider/Unreal_Engine__Debugger.html)
- [_Rider can't provide user-friendly views of Unreal Engine data types when debugging_ @ rider-support.jetbrains.com 2022](https://rider-support.jetbrains.com/hc/en-us/community/posts/4417460499474-Rider-can-t-provide-user-friendly-views-of-Unreal-Engine-data-types-when-debugging)
