Unreal Build Tool is Epic Games compiler driver for building Unreal Engine projects.
It is a C# program that reads `MyProject.uproject` files, `MyProject.Target.cs` files, and `MyModule.Build.cs` files.

Unreal Build Tool is run by running the `RunUBT.sh` script in the Unreal Engine installation.
```shell
${UNREAL_ROOT}/Engine/Build/BatchFiles/RunUBT.sh
```

It has a bunch of flags and options for various actions it can perform.
For example to generate [[compile_commands.json]] files with run with the `-Mode=GenerateClangDatabase` flag.


[Full command](https://discord.com/channels/187217643009212416/375022233875382274/1482670247717634078):
```shell
# Generate project files.
${UNREAL_ROOT}/Engine/Build/BatchFiles/Linux/GenerateProjectFiles.sh \
  -project=(realpath *.uproject) \
  -game \
  -engine \
  -makefile \
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

