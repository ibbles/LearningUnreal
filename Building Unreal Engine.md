Before the following steps can be done there is a bit of one-time setup that has to be done.
See instructions at https://docs.unrealengine.com/en-US/Platforms/Linux/BeginnerLinuxDeveloper/SettingUpAnUnrealWorkflow/index.html.

The basic steps for building Unreal Engine on Linux are as follows, where
- `$VERSION2`: A two-part Unreal Engine version number, such as `5.0`.
- `$VERSION3`: A three-part Unreal Engine version number, such as `5.0.1`.

- `mkdir UnrealEngine_$VERSION2
- `cd UnrealEngine_$VERSION2`
- `git clone --depth 1 https://github.com/EpicGames/UnrealEngine.git -b $VERSION3-release .`
- Fix `Build.version`. See below.
- Fix `UEBuildModuleCPP.cs`. See below.
- `./Setup.sh`
- `./GenerateProjectFiles.sh`
- `make
- Optionally create an [[Installed Build]].


# Asset version compatibility, `Build.version`

Unreal Engine differentiates between official binary releases and custom source builds.
When we build ourselves we are creating a custom source build even if we haven't made any changes to the engine.
Epic Games only guarantees asset compatibility between official binary releases, since a source build might have been mucking about with asset formats and layouts.
We can promise that our custom source build is compatible with a particular official binary release by writing that release's Changelist number to `Engine/Build/Build.version`.
The numbers to enter varies depending on the Unreal Engine version we are building.
You can find the numbers to enter by looking at that same file in an installation from an official binary release.

- **Unreal Engine 4.25**
    - Changelist: 14469661
    - CompatibleChangelist: 13144385
 - **Unreal Engine 4.26**
    - Changelist: 15973114
    - CompatibleChangelist: 14830424
 - **Unreal Engine 4.27**
    - Changelist: 18319896
    - CompatibleChangelist: 17155196
 - **Unreal Engine 5.0**
    - Changelist: 19505902
    - CompatibleChangelist: 19505902

For some reason the Changelist numbers I see in official Windows binary releases doesn't match with what I see at [adamrehn/ue4-docker @ github.com](https://github.com/adamrehn/ue4-docker/blob/8ba1d5f8a873f76583a32f61e5399827193f0e11/ue4docker/infrastructure/BuildConfiguration.py#L31-L40).
Makes me question whether it is safe to copy these numbers from the Windows installations.
The Docker images uses the Comaptible Changelist also for the Changelist.


# Preprocessor defined, `UEBuildModuleCPP.cs`

`GenerateProjectFiles.sh` generates a bunch of information needed by the IDE to interpret the project properly.
This code has bugs in it that causes some of that information to be lost, causing massive amounts of errors when the IDE tries to parse the Unreal Engine code.
We can fix this by editing `UEBuildModuleCPP.cs`.
The changes necessary depend on the Unreal Engine version being built.

Patches are applied either with `git apply $PATCH_FILE` or `patch -p1 < $PATCH_FILE`.
`git apply` can be dry-run by adding `--check` after `apply`.


## Unreal Engine 4.25

This change set is from https://gist.github.com/ericwomer/142650e65473087073f30e5fb97fd6e8.

```diff
diff --git a/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs b/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
index 4c0428ad373..6a6d4f71397 100644
--- a/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
+++ b/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
@@ -404,6 +404,10 @@ namespace UnrealBuildTool
 
 			// Write all the definitions to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, null, Graph);
+			if (CompileEnvironment.Definitions.Count > 0)
+			{
+				CompileEnvironment.Definitions.Clear();
+			}
 
 			// Mapping of source file to unity file. We output this to intermediate directories for other tools (eg. live coding) to use.
 			Dictionary<FileItem, FileItem> SourceFileToUnityFile = new Dictionary<FileItem, FileItem>();
@@ -874,6 +878,10 @@ namespace UnrealBuildTool
 		{
 			// Write all the definitions out to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, "Adaptive", Graph);
+			if (CompileEnvironment.Definitions.Count > 0)
+			{
+				CompileEnvironment.Definitions.Clear();
+			}
 
 			// Compile the files
 			return ToolChain.CompileCPPFiles(CompileEnvironment, Files, IntermediateDirectory, ModuleName, Graph);
@@ -886,6 +894,10 @@ namespace UnrealBuildTool
 
 			// Write all the definitions out to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, "Adaptive", Graph);
+			if (CompileEnvironment.Definitions.Count > 0)
+			{
+				CompileEnvironment.Definitions.Clear();
+			}
 
 			// Compile the files
 			return ToolChain.CompileCPPFiles(CompileEnvironment, Files, IntermediateDirectory, ModuleName, Graph);
@@ -1001,7 +1013,7 @@ namespace UnrealBuildTool
 						}
 
 						CompileEnvironment = new CppCompileEnvironment(CompileEnvironment);
-						CompileEnvironment.Definitions.Clear();
+						// CompileEnvironment.Definitions.Clear(); // @todo: Do not clear definitions prior to end of their usefulness
 						CompileEnvironment.ForceIncludeFiles.Add(PrivateDefinitionsFileItem);
 						CompileEnvironment.PrecompiledHeaderAction = PrecompiledHeaderAction.Include;
 						CompileEnvironment.PrecompiledHeaderIncludeFilename = Instance.HeaderFile.Location;
@@ -1039,7 +1051,7 @@ namespace UnrealBuildTool
 				using (StringWriter Writer = new StringWriter())
 				{
 					WriteDefinitions(CompileEnvironment.Definitions, Writer);
-					CompileEnvironment.Definitions.Clear();
+					//CompileEnvironment.Definitions.Clear(); @todo: Do not clear definitions prior to end of their usefulness
 
 					FileItem PrivateDefinitionsFileItem = Graph.CreateIntermediateTextFile(PrivateDefinitionsFile, Writer.ToString());
 					CompileEnvironment.ForceIncludeFiles.Add(PrivateDefinitionsFileItem);
```


# Unreal Engine 4.26

This change set based on https://gist.github.com/jerobarraco/92839db6e6305fb04a04bab415ec8ae4. I can't use that as-is due to
```shell
/media/s2000/UnrealEngine/git/UnrealEngine_4.26
⎇  4.26.2-release➤ git apply --check ue4_26-defines-fix.patch
error: corrupt patch at line 18

/media/s2000/UnrealEngine/git/UnrealEngine_4.26
⎇  4.26.2-release➤ patch -p1 < ue4_26-defines-fix.patch
patching file Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
patch: **** malformed patch at line 17:
```

What follows is my own version, manually applied and the diff generated with `git diff Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs > ue_4.26_defines_fix.diff`.
Similar goes for Unreal Engine 4.27 and 5.0 as well.

```diff
diff --git a/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs b/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
index ad61e4bdf..f7a948d8b 100644
--- a/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
+++ b/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
@@ -437,6 +437,12 @@ namespace UnrealBuildTool
 			// Write all the definitions to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, null, Graph);
 
+			// Defines patch.
+			if (CompileEnvironment.Definitions.Count > 0)
+			{
+				CompileEnvironment.Definitions.Clear();
+			}
+
 			// Mapping of source file to unity file. We output this to intermediate directories for other tools (eg. live coding) to use.
 			Dictionary<FileItem, FileItem> SourceFileToUnityFile = new Dictionary<FileItem, FileItem>();
 
@@ -923,6 +929,12 @@ namespace UnrealBuildTool
 			// Write all the definitions out to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, "Adaptive", Graph);
 
+			// Defines patch.
+			if (CompileEnvironment.Definitions.Count > 0)
+			{
+				CompileEnvironment.Definitions.Clear();
+			}
+
 			// Compile the files
 			return ToolChain.CompileCPPFiles(CompileEnvironment, Files, IntermediateDirectory, ModuleName, Graph);
 		}
@@ -935,6 +947,11 @@ namespace UnrealBuildTool
 			// Write all the definitions out to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, "Adaptive", Graph);
 
+			// Defines patch.
+			if (CompileEnvironment.Definitions.Count > 0){
+				CompileEnvironment.Definitions.Clear();
+			}
+
 			// Compile the files
 			return ToolChain.CompileCPPFiles(CompileEnvironment, Files, IntermediateDirectory, ModuleName, Graph);
 		}
@@ -1005,7 +1022,10 @@ namespace UnrealBuildTool
 					PrecompiledHeaderInstance Instance = CreatePrivatePCH(ToolChain, FileItem.GetItemByFileReference(FileReference.Combine(ModuleDirectory, Rules.PrivatePCHHeaderFile)), CompileEnvironment, Graph);
 
 					CompileEnvironment = new CppCompileEnvironment(CompileEnvironment);
-					CompileEnvironment.Definitions.Clear();
+
+					// Defines patch.
+					// CompileEnvironment.Definitions.Clear();
+
 					CompileEnvironment.PrecompiledHeaderAction = PrecompiledHeaderAction.Include;
 					CompileEnvironment.PrecompiledHeaderIncludeFilename = Instance.HeaderFile.Location;
 					CompileEnvironment.PrecompiledHeaderFile = Instance.Output.PrecompiledHeaderFile;
@@ -1049,7 +1069,10 @@ namespace UnrealBuildTool
 						}
 
 						CompileEnvironment = new CppCompileEnvironment(CompileEnvironment);
-						CompileEnvironment.Definitions.Clear();
+
+						// Defines patch.
+						// CompileEnvironment.Definitions.Clear();
+
 						CompileEnvironment.ForceIncludeFiles.Add(PrivateDefinitionsFileItem);
 						CompileEnvironment.PrecompiledHeaderAction = PrecompiledHeaderAction.Include;
 						CompileEnvironment.PrecompiledHeaderIncludeFilename = Instance.HeaderFile.Location;
```


## Unreal Engine 4.27

```diff
diff --git a/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs b/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
index 481bb6db1..96457a88c 100644
--- a/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
+++ b/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
@@ -437,6 +437,12 @@ namespace UnrealBuildTool
 			// Write all the definitions to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, null, Graph);
 
+			// Defines patch.
+			if (CompileEnvironment.Definitions.Count > 0)
+			{
+				CompileEnvironment.Definitions.Clear();
+			}
+
 			// Mapping of source file to unity file. We output this to intermediate directories for other tools (eg. live coding) to use.
 			Dictionary<FileItem, FileItem> SourceFileToUnityFile = new Dictionary<FileItem, FileItem>();
 
@@ -923,6 +929,12 @@ namespace UnrealBuildTool
 			// Write all the definitions out to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, "Adaptive", Graph);
 
+			// Defines patch.
+			if (CompileEnvironment.Definitions.Count > 0)
+			{
+				CompileEnvironment.Definitions.Clear();
+			}
+
 			// Compile the files
 			return ToolChain.CompileCPPFiles(CompileEnvironment, Files, IntermediateDirectory, ModuleName, Graph);
 		}
@@ -935,6 +947,11 @@ namespace UnrealBuildTool
 			// Write all the definitions out to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, "Adaptive", Graph);
 
+			// Defines patch.
+			if (CompileEnvironment.Definitions.Count > 0){
+				CompileEnvironment.Definitions.Clear();
+			}
+
 			// Compile the files
 			return ToolChain.CompileCPPFiles(CompileEnvironment, Files, IntermediateDirectory, ModuleName, Graph);
 		}
@@ -1005,7 +1022,10 @@ namespace UnrealBuildTool
 					PrecompiledHeaderInstance Instance = CreatePrivatePCH(ToolChain, FileItem.GetItemByFileReference(FileReference.Combine(ModuleDirectory, Rules.PrivatePCHHeaderFile)), CompileEnvironment, Graph);
 
 					CompileEnvironment = new CppCompileEnvironment(CompileEnvironment);
-					CompileEnvironment.Definitions.Clear();
+
+					// Defines patch.
+					// CompileEnvironment.Definitions.Clear();
+
 					CompileEnvironment.PrecompiledHeaderAction = PrecompiledHeaderAction.Include;
 					CompileEnvironment.PrecompiledHeaderIncludeFilename = Instance.HeaderFile.Location;
 					CompileEnvironment.PrecompiledHeaderFile = Instance.Output.PrecompiledHeaderFile;
@@ -1049,7 +1069,10 @@ namespace UnrealBuildTool
 						}
 
 						CompileEnvironment = new CppCompileEnvironment(CompileEnvironment);
-						CompileEnvironment.Definitions.Clear();
+
+						// Defines patch.
+						// CompileEnvironment.Definitions.Clear();
+
 						CompileEnvironment.ForceIncludeFiles.Add(PrivateDefinitionsFileItem);
 						CompileEnvironment.PrecompiledHeaderAction = PrecompiledHeaderAction.Include;
 						CompileEnvironment.PrecompiledHeaderIncludeFilename = Instance.HeaderFile.Location;
```

## Unreal Engine 5.0

```diff
diff --git a/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs b/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
index 309c435df..18bf34514 100644
--- a/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
+++ b/Engine/Source/Programs/UnrealBuildTool/Configuration/UEBuildModuleCPP.cs
@@ -503,6 +503,12 @@ namespace UnrealBuildTool
 			// Write all the definitions to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, null, Graph);
 
+			// Defines patch.
+			if (CompileEnvironment.Definitions.Count > 0)
+			{
+				CompileEnvironment.Definitions.Clear();
+			}
+
 			// Mapping of source file to unity file. We output this to intermediate directories for other tools (eg. live coding) to use.
 			Dictionary<FileItem, FileItem> SourceFileToUnityFile = new Dictionary<FileItem, FileItem>();
 
@@ -1011,6 +1017,12 @@ namespace UnrealBuildTool
 			// Write all the definitions out to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, "Adaptive", Graph);
 
+			// Defines patch.
+			if (CompileEnvironment.Definitions.Count > 0)
+			{
+				CompileEnvironment.Definitions.Clear();
+			}
+
 			// Compile the files
 			return ToolChain.CompileCPPFiles(CompileEnvironment, Files, IntermediateDirectory, ModuleName, Graph);
 		}
@@ -1023,6 +1035,11 @@ namespace UnrealBuildTool
 			// Write all the definitions out to a separate file
 			CreateHeaderForDefinitions(CompileEnvironment, IntermediateDirectory, "Adaptive", Graph);
 
+			// Defines patch.
+			if (CompileEnvironment.Definitions.Count > 0){
+				CompileEnvironment.Definitions.Clear();
+			}
+
 			// Compile the files
 			return ToolChain.CompileCPPFiles(CompileEnvironment, Files, IntermediateDirectory, ModuleName, Graph);
 		}
@@ -1099,7 +1116,10 @@ namespace UnrealBuildTool
 					PrecompiledHeaderInstance Instance = CreatePrivatePCH(ToolChain, FileItem.GetItemByFileReference(FileReference.Combine(ModuleDirectory, Rules.PrivatePCHHeaderFile)), CompileEnvironment, Graph);
 
 					CompileEnvironment = new CppCompileEnvironment(CompileEnvironment);
-					CompileEnvironment.Definitions.Clear();
+
+					// Defines patch.
+					// CompileEnvironment.Definitions.Clear();
+
 					CompileEnvironment.PrecompiledHeaderAction = PrecompiledHeaderAction.Include;
 					CompileEnvironment.PrecompiledHeaderIncludeFilename = Instance.HeaderFile.Location;
 					CompileEnvironment.PrecompiledHeaderFile = Instance.Output.PrecompiledHeaderFile;
@@ -1143,7 +1163,10 @@ namespace UnrealBuildTool
 						}
 
 						CompileEnvironment = new CppCompileEnvironment(CompileEnvironment);
-						CompileEnvironment.Definitions.Clear();
+
+						// Defines patch.
+						// CompileEnvironment.Definitions.Clear();
+
 						CompileEnvironment.ForceIncludeFiles.Add(PrivateDefinitionsFileItem);
 						CompileEnvironment.PrecompiledHeaderAction = PrecompiledHeaderAction.Include;
 						CompileEnvironment.PrecompiledHeaderIncludeFilename = Instance.HeaderFile.Location;
```


# Address Sanitizer

Address Sanitizer is a compiler option that instruments the compiled binary with extra checks around memory operations.
Helps find mismatched free/delete, double deletes, and some out-of-bounds accesses.

Unreal Engine can be built with Address Sanitizer support.
```bash
UE_ROOT$ make UnrealEditor ARGS=-EnableASan
```

Then build your project with the Address Sanitizer enabled engine.
```bash
PROJECT_ROOT$ ./Engine/Build/BatchFiles/Linux/Build.sh
	Development
	Linux
	*.uproject
	-TargetType=Editor
	-EnableASan
```

Then open your project
```bash
$ $UE_ROOT/Engine/Binaries/Linux/UnrealEditor $PROJECT_ROOT/*.uproject
```

May need a system install of LLVM and a symbolizer path.
```bash
$ sudo apt install llvm
$ export ASAN_SYMBOLIZER_PATH=/usr/lib/llvm-9/bin/llvm-symbolizer  # Path may be different.
```

