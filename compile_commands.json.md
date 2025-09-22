[[Unreal Build Tool]] can generate a `compile_commands.json` file which is used by many C++ IDEs.

```shell
/home/unreal_projects/my_project
$ ${UNREAL_ROOT}/Engine/Build/BatchFiles/RunUBT.sh \
	-Project=(realpath *.uproject) \
	-Mode=GenerateClangDatabase \
	-Target=MyProjectEditor \
	Development \
	Linux
```

# References

- 1: [_Can you use UBT to create a compile_commands.json for Unreal Source files?_ @ reddit.com 2024](https://www.reddit.com/r/unrealengine/comments/1am9zfd/can_you_use_ubt_to_create_a_compile_commandsjson/)
