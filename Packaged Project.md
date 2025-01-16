A packaged project is runtime version of the Unreal Engine project.
It is the executable that is shipped to users.
It contains [[Asset]]s in [[Cooking|Cooked]] form.

# Package From Unreal Editor

To package a project do [[Main Tool Bar]] > Platforms > `PLATFORM` > Package Project.
`PLATFORM` is e.g. Linus or Windows.  (Win64? Not sure what the entry says.)
Select a directory where the packaged project should be placed.
Should be an empty directory.

Can set a Build Configuration to package with, such as Shipping, Development, or DebugGame.

Starting with Unreal Engine 5.5 it is necessary to run the following command in a terminal first:
```shell
$UE_ROOT/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux Shipping \
	-Project=PROJECT_PATH
	-TargetType=Game
```

`PROJECT_PATH` is the full path to the project's `*.uproject` file.


# Package From Command Line

I found this somewhere.
I don't know what all the flags do.

TODO Read about these flags.

```shell
"$UE_ROOT/Engine/Build/BatchFiles/RunUAT.sh" \
	-ScriptsForProject="MyUnrealProjects/MyProject/MyProject.uproject" \
	Turnkey \
	-command=VerifySdk \
	-platform=Linux \
	-UpdateIfNeeded \
	-EditorIO -EditorIOPort=48583 \
	-project="/MyUnrealProjects/MyProject/MyProject.uproject" \
	BuildCookRun \
	-nop4 \
	-utf8output \
	-nocompileeditor \
	-skipbuildeditor \
	-cook  \
	-project="/MyUnrealProjects/MyProject/MyProject.uproject" \
	-target=CPPPackageTest \
	-unrealexe="MyUnrealInstallation/Engine/Binaries/Linux/UnrealEditor-Linux-DebugGame" \
	-platform=Linux \
	-stage \
	-archive \
	-package \
	-build \
	-pak \
	-iostore \
	-compressed \
	-prereqs \
	-archivedirectory="/MyPackagedProjects/MyProject/" \
	-clientconfig=Shipping \
	-nodebuginfo \
	-nocompile \
	-nocompileuat
```


# Running Packaged Project

If your project is named `MyGame` then run the packaged project with
```shell
➤ ./Yumi.sh -Windowed -ForceRes -ResX=1920 -ResY=1080 -VSync=On
```
if you want a 1920x1080 window with [[VSync]] enabled.

`-VSync=On` does not work in Unreal Engine 5.3. Or 5.4
I worry that it may have been an illusion that it ever worked.


# Troubleshooting

Sometimes packaging fails without error message.
Try increasing (or decreasing) the file descriptor limit.

```shell
ulimit -Hn 524288
```

The above will update for the current shell only.
Update `/etc/security/limits.d/` or `/etc/security/limits.conf` to make it permanent.
