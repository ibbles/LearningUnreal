Also called CVar for short.
Set from the console, which is opened with `~`.
Or Top Menu Bar > Windows > Developer Tools > Output Log > Input box at the bottom.

Type `r.` into the console and you will get a list of a whole lot of things that can be set.
These are the CVars.
The `r` stands for Rendering.
Tweaking these variables is a way to provide scalability and control.
Can configure these to be set differently depending on device or graphics setting category.
See [[Device Profiles]].

Entering the name of a CVar without a value will print the current value and where that value comes from.
Entering the name of a CVar with a value will set that variable to that value.

Many CVars are named Quality.
Setting a Quality CVar to 0 turns that feature off.

Can be used to for a minimum [[Mesh LOD]].
Can be used to force a Material quality, a different path out of a static branch node in the Material Graph.
Can be used to force particle system to run at a lower LOD.
Can be used to set a distance multiplier for all LODs, pushing the low-quality transitions closer.
Can be used to set a foliage density multiplier to reduce the amount of grass.

Unreal Engine can estimate the computational power of the machine it is running on and automatically set some of these settings.
(
How do I make it do that?
)

Changes made in the console are not persistent.
Restarting Unreal Editor will reset them to their `.ini` configured values, or the default.

There are also `show` commands.
Such as `show postprocessing` and `show bloom`.
Not sure what this does.


Type `help` to generate a HTML help file.
Lists all the available rendering commands, including descriptions.


# Setting CVars

The quick-and-easy way to set a CVar is to type its name and new value into the console.
For example
```
r.Streaming.PoolSize 4096
```
This way creates a temporary setting that only exists for the current run of the editor or game, i.e. for this process only.

CVars can be set in `.ini` files.
There are different `.ini` files for different categories of CVars.
If you know the section name for the CVar you are after then you can search for that in the output log.
Will show which `.ini` file the engine has loaded CVars from.

CVars can also be driven by game play logic, for example through [[Blueprint Visual Script]].
Use the node Execute Console Command and type the CVar name and new value just like you would in the regular console.
In this way we can turn engine features on and off dynamically, and we can tie specific settings to specific levels through the Level Blueprint and the Begin Play [[Blueprint Event]].
CVars set in Begin Play will only be set once the level is played, not when only opened in the editor.
The new value will persist through the [[Play In Editor]] session and remain afterwards.


# Interesting CVars

## Rendering Features
- `r.screenPercentage <NUMBER>`: Do up- or down scaling. 100% means native, the rendered image matches the screen pixels in the viewport. Above 100% renders more pixels than are in the viewport, i.e. super-sampling. This increases the visual quality at the cost of a higher rendering time. Below 100% renders fewer pixels than are in the viewport, i.e. up-scaling. This increases rendering performance at the cost of image quality.

## Textures
- `r.Streaming.PoolSize`: The size in MiB to allocate for streaming textures. Increase this if you get blurry textures and a warning the in the viewport about the texture streaming pool being over budget. If you have VRAM to spare.
- `r.VT.Borders`: Render borders around [[Virtual Texture]] tiles.

## Foliage
- `foliage.ForceLOD <NUMBER>`: Render all [[Foliage]] at this LOD.
	- Not sure what happens if that LOD level isn't available for a particular [[Static Mesh Foliage]].



# Custom CVars

We can add our own console variables.

```cpp
namespace MyConsoleVariables
{
    static TAutoConsoleVariable CVarMyConsoleVariable(
        TEXT("mygame.MyConsoleVariable"),
        0, // Default value.
        TEXT("My console variable."),
        ECVF_Default);
}


UMySubsystem::Initialize()
{
    MyConsoleVariables::CVarMyConsoleVariable->OnChangedDelegate().AddWeakLambda(
    this, [this](const IConsoleVariable* CVar)
    {
        if (!CVar->IsVariableInt())
        {
            return;
        }
        int32 Value = CVar->GetInt();
        // Use Value.
    });
}
```


# References

- [_Console Variables in C++_ by Epic Games @ dev.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/console-variables-cplusplus-in-unreal-engine?application_version=5.4)

