A packaged project is runtime version of the Unreal Engine project.
It is the executable that is shipped to users.
It contains [[Asset]]s in [[Cooking|Cooked]] form.

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