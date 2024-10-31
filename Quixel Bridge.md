

**As of Unreal Engine 5.4 Quixel Bridge does not exist anymore. Use the Fab plugin instead.**


Quixel Bridge is a tool for importing assets from the [[Quixel Megascans]] library into an Unreal Engine project.
There is a stand-alone application and an Unreal Engine plugin.
Starting with Unreal Engine 5 .0  the Quixel Bridge plugin is shipped with source builds of the engine, but not [[Installed Build]] which means the official [Epic Games binaries](www.unrealengine.com/en-US/linux) also don't have it, though there is a separate Zip archive download for the Quixel Bridge plugin for each engine version.
See _Installation_ below for how to add the plugin to an [[Installed Build]].


# Plugin Usage

Assets can be imported with Content Browser > right-click > Add Quixel Content. Quixel Bridge opens.
Or the Add menu from the [[Top Menu Bar]].

If the editor crashes immediately when opening the Quixel Bridge window then check the execute permissions on `Engine/Binaries/Linux/EpicWebHelper`.
At least Unreal Engine 5.1.1 has a bug where it was missing.
```shell
ls -l $UE_ROOT/Engine/Binaries/Linux/EpicWebHelper
# Check the permission flags, the 'x' one should be enabled.
# If not:
chmod +x $UE_ROOT/Engine/Binaries/Linux/EpicWebHelper
```

Use the search bar at the top of the tree view to the left to find assets to import.
The assets are grouped into a number of categories, such as [[Quixel Megascans Surfaces|Surfaces]] and [[Quixel Megascans 3D Assets|3D Assets]].
Quixel Megascans assets imported imported from Quixel Bridge are stored in the `Megascans` folder of the Content Browser.


# Installation

## Unreal Engine 4

For Unreal Engine 4 the Quixel Bridge plugin is installed from the Quixel Bridge stand-alone application.
It think it can be done from Top Menu Bar > Edit > Export Settings > Export Target.

## Unreal Engine 5 Source Build
For a source build of Unreal Engine 5 there is no install step, the plugin is included.


## Unreal Engine 5 Installed Build

This entire section is mostly outdated.
The [Unreal Engine download page for Linux](www.unrealengine.com/en-US/linux) does provide Zip archives for the plugin.
Extract into your Unreal Engine installation directory.


For an [[Installed Build]], such as the official [Epic Games binaries](www.unrealengine.com/en-US/linux), one has to build the plugin separately.
I haven't been able to find how to download the plugin for Unreal Engine 5, the [plugin downloads page](https://quixel.com/plugins/) currently (2022-07-27) only contains Unreal Engine 4 variants.
Instead I clone the Unreal Engine repository.
Make sure you checkout the same release tag as the version of the official binaries you have.
Then use the official binaries installation to build the Bridge plugin in the Git repository.

The following commands are run from the [[Installed Build]] directory and the Unreal Engine working copy is at `/ue-git`.
Adjust your path accordingly.
```bash
$ ./Engine/Build/BatchFiles/RunUAT.sh \
	BuildPlugin \
	-Plugin=/ue-git/Engine/Plugins/Bridge/Bridge.uplugin \
	-Package=/tmp/Bridge
$ mv /tmp/Bridge ./Engine/Plugins/
```

Start Unreal Editor and enable the Bridge plugin in > Main Tool Bar > Settings > Plugins.
Restart Unreal Editor.


# Asset Quality

Quixel Megascans assets are available in multiple quality levels.


# Settings

Clicking the settings button (sliders / hamburger with dots) next to the quality drop-down opens the settings window.

The settings window contains the Create Material Blend button.
This creates a blend [[Material]] from the Quixel Surface Assets currently selected in the Content Browser.
For more information see [[Quixel Megascans Surfaces]] and [[Quixel Megascans Surfaces Material Blend]].
