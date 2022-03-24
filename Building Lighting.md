Bake lighting with Main Tool Bar > Build > Build Lighting Only .

[[Baked Lighting]]


# Command line

The following command is supposed to build lighting for a level, but it errors out for me.

```shell
$UE_BIN/UE4Editor-Cmd \
	(readlink -f *.uproject) -run=resavepackages
	-buildlighting
	-quality=Medium
	-allowcommandletrendering
	-map=$MAP_NAME
```

You can build for all maps by leaving out the `-map=$MAP_NAME` part.



The error:
```
LogInit: Error: Timed out waiting for the recipient (TimeWaitingSec = 60.011036)
CurrentThread == GetCurrentThread() [File:/media/s800/UnrealEngine_4.25/Engine/Source/Runtime/Core/Private/Async/TaskGraph.cpp] [Line: 1417]
LogContentCommandlet: Error: [REPORT] Failed building lighting for /Game/Levels/MyLevel
```
