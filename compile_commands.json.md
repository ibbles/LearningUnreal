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

[Another option](https://discord.com/channels/187217643009212416/375022233875382274/1482670247717634078):
```shell
# Generate project files.
${UNREAL_ROOT}/Engine/Build/BatchFiles/Linux/GenerateProjectFiles.sh \
  -project=(realpath *.uproject) \
  -game \
  -engine \
  -makefile 
  -clangd

# Generate Clang database.
${UNREAL_ROOT}/Engine/Build/BatchFiles/Linux/Build.sh \
	MyProjectEditor \
	Linux \
	Development \
	-project=(realpath *.uproject) \
	-mode=GenerateClangDatabase \
	-allmodules
```



# References

- 1: [_Can you use UBT to create a compile_commands.json for Unreal Source files?_ @ reddit.com 2024](https://www.reddit.com/r/unrealengine/comments/1am9zfd/can_you_use_ubt_to_create_a_compile_commandsjson/)
