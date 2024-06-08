A Commandlet is a way to run Unreal Editor commands / tasks from the command line.
Meant for scripting and batch processing on a build machine.
Does things like:
- Compile all [[Blueprint Class]]es.
- Save all [[Texture]]s.
- [[World Partition]] conversion.
	- Converting a non-[[World Partition]] [[Level]] to using [[World Partition]].
- And much more.

Commands follow this structure:
```
 unreal_executable project_name map_name tasks arguments
```

A few examples follow.

Generate [[World Partition]] mini map:
```
$UE_EDITOR \
	MyProject.uproject \
	/Game/MyProject/Maps/MyMap \
	-Run=WorldPartitionBuilderCommandlet \
	-AllowCommandletRendering \
	-Builder=WorldPartitionMiniMapBuilder
```

Generate World Partition [[Hierarchical Level Of Detail HLOD]]:
```
$UE_EDITOR \
	MyProject.uproject \
	/Game/MyProject/Maps/MyMap \
	-Run=WorldPartitionBuilderCommandlet \
	-AllowCommandletRendering \
	-Builder=WorldPartitionHLODsBuilder
```

# References

- [_Building Bigger: Changing Your Workflow for Building Worlds instead of Scenes | Unreal Fest 2023_ by Chris Murphy @ dev.epicgames.com/talks-and-demos 2024 UE5.3](https://dev.epicgames.com/community/learning/talks-and-demos/jwlJ/unreal-engine-building-bigger-changing-your-workflow-for-building-worlds-instead-of-scenes-unreal-fest-2023)

