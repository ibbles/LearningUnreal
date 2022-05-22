Creating an Installed Build is an optional step when [[Building Unreal Engine]].

```bash
./Engine/Build/BatchFiles/RunUAT.sh
	BuildGraph
	-Target="Make Installed Build Linux"
	-Script=Engine/Build/InstalledEngineBuild.xml
	-Set:HostPlatformOnly=true
	-Set:WithDDC=false
	-Set:WithLinuxAArch64=false  # For Unreal Engine 4.
	-Set:WithLinuxArm64=false  # For Unreal Engine 5.
```


Unreal Engine 4 single line.
```
./Engine/Build/BatchFiles/RunUAT.sh BuildGraph -Target="Make Installed Build Linux" -Script=Engine/Build/InstalledEngineBuild.xml -Set:HostPlatformOnly=true -Set:WithDDC=false -Set:WithLinuxAArch64=false
```

Unreal Engine 5 single line.
```
./Engine/Build/BatchFiles/RunUAT.sh	BuildGraph	-Target="Make Installed Build Linux"	-Script=Engine/Build/InstalledEngineBuild.xml	-Set:HostPlatformOnly=true	-Set:WithDDC=false	-Set:WithLinuxArm64=false
```