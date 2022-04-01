Short for Console Variables.
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

Changes made in the console are not persistent.
Restarting Unreal Editor will reset them to their `.ini` configured values, or the default.

There are also `show` commands.
Such as `show postprocessing` and `show bloom`.
Not sure what this does.


Type `help` to generate a HTML help file.
Lists all the available rendering commands, including descriptions.
