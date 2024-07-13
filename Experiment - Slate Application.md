This note contains my attempts to build a Slate application.


# UnrealSlateAppTemplate

There is a Slate App template on GitHub, [_UnrealSlateAppTemplate_](https://github.com/lpestl/UnrealSlateAppTemplate),  that demonstrates how to create a Slate application.
```shell
/media/s1300/unreal_projects
➤ git clone git@github.com:lpestl/UnrealSlateAppTemplate.git

# The .uproject file didn't have Engine Association set, so I added
#  "EngineAssociation": "5.4",
# to the file.

/media/s1300/unreal_projects/UnrealSlateAppTemplate  
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Editor
# Snip.
Targets with a unique build environment cannot be built with an installed engine.
```

Why not?

https://forums.unrealengine.com/t/targets-with-a-unique-build-environment-cannot-be-built-with-an-installed-engine/1353217 suggests two possible solutions:
- Remove `BuildEnvironment = TargetBuildEnvironment.Unique;` from the `.Taget.cs` files.
- Delete the `Editor.Target.cs` file.

In this case, there is no `BuildEnvironment` at all in any `.Target.cs`, and there is no `Editor.Target.cs`.
I have seen this error before.
At that time I fixed it by adding `DefaultBuildSettings = BuildSettingsVersion.Latest;` to `.Target.cs`.
Trying that.
No, that has already been set.

https://stackoverflow.com/questions/71535060/how-do-i-set-the-program-type hints that the problem is the `Type = TargetType.Program` that is the problem.
Suggests changing it to `TargetType.Game`.
Testing.

```shell
/media/s1300/unreal_projects/UnrealSlateAppTemplate
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Editor

Could not find definition for module 'EmptyProject', (referenced via UnrealEditor.Target.cs)
```

Let's not build `-TargetType=Editor`.
```shell
/media/s1300/unreal_projects/UnrealSlateAppTemplate
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux  \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Game
# Snip.
UnrealSlateAppTemplate modifies the values of properties:
[
	bCompileAgainstEngine
].
This is not allowed, as UnrealSlateAppTemplate has build products in common with UnrealGame.
- Remove the modified setting,
- change UnrealSlateAppTemplate to use a unique build environment by setting 'BuildEnvironment = TargetBuildEnvironment.Unique;' in the UnrealSlateAppTemplateTarget constructor,
- or
- set bOverrideBuildEnvironment = true to force this setting on.
```

The `.Target.cs` file does indeed set `bCompileAgainstEngine` to false.
Let's not do that, commenting that line out.

```shell
/media/s1300/unreal_projects/UnrealSlateAppTemplate
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Game
# Snip.
Could not find definition for module 'EmptyProject', (referenced via UnrealSlateAppTemplate.Target.cs)
```

`EmptyProject` is the name of the project's main module.
Declared as follows in the `.uproject` file:
```js
	"Modules": [
		{
			"Name": "EmptyProject",
			"Type": "Runtime",
			"LoadingPhase": "Default"
		}
	]
```

But there is no directory in the `Source` directory with this name.
There is a folder named `UnrealSlateAppTemplate`.
Changing the `.uproject` `"name"` attribute to that.

```shell
/media/s1300/unreal_projects/UnrealSlateAppTemplate  
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux  \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Game
# Snip.
Missing precompiled manifest for 'StandaloneRenderer', '/media/s1300/unreal_engine/5.4.2/Engine/Intermediate/Build/Linux/UnrealGame/Development/StandaloneRenderer/StandaloneRenderer.precompiled'. This module was most likely not flagged for being included in a precompiled build - set 'PrecompileForTargets = PrecompileTargetsType.Any;' in StandaloneRenderer.build.cs to override. If part of a plugin, also check if its 'Type' is correct.
```

OK.
What is a "precompiled manifets" and should "StandaloneRenderer" have one or not?
It suggests editing the engine installation, but I'm not going to do that.

Is this because the project's `.Target.cs` sets `LinkType = TargetLinkType.Monolithic;`?
Commenting that line out and trying again.

```shell
/media/s1300/unreal_projects/UnrealSlateAppTemplate[6]
⎇  main➤ /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/UnrealSlateAppTemplate/UnrealSlateAppTemplate.uproject \
	-TargetType=Game
# Snip
Missing precompiled manifest for 'StandaloneRenderer', '/media/s1300/unreal_engine/5.4.2/Engine/Intermediate/Build/Linux/UnrealGame/Development/StandaloneRenderer/StandaloneRenderer.precompiled'. This module was most likely not flagged for being included in a precompiled build - set 'PrecompileForTargets = PrecompileTargetsType.Any;' in StandaloneRenderer.build.cs to override. If part of a plugin, also check if its 'Type' is correct.
```

Same error.

Maybe the Requirements list in the template [README](https://github.com/lpestl/UnrealSlateAppTemplate?tab=readme-ov-file#requirements) is correct in that a from-sources build of Unreal Engine is required.
That would be disappointing.

So.
Maybe we will never be able to build this application, Unreal Engine just isn't built to support that.
I can still read the code though.
Maybe I'll learn something from that, enough to replicate it in a "regular" (whatever that means) Unreal Engine project.


# UE4 Plugin for Scene Capture rendering to separate window

URL: https://github.com/Batname/UE4MultiWindow/tree/master
Author: Batname

While this looked good, it is editor-only. Not what I need.
There is a [fork](https://github.com/Batname/UE4MultiWindow/compare/master...raptoravis:UE4MultiWindow:master) that tries to make it non-editor, but I don't think it was taken all the way.
Testing.
```shell
/media/s1300/unreal_projects/others
➤ git clone git@github.com:raptoravis/UE4MultiWindow.git

/media/s1300/unreal_projects/others/UE4MultiWindow
⎇  master➤ sed -i \
	's,"EngineAssociation":.*,"EngineAssociation": "5.4"\,,g' \
	*.uproject
⎇  master➤  /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/others/UE4MultiWindow/UE4MultiWindow.uproject \
	-TargetType=Editor
# Snip.
UE4MultiWindowEditor modifies the values of properties:
[ bStrictConformanceMode ].
This is not allowed, as UE4MultiWindowEditor has build products in common with UnrealEditor.
Remove the modified setting, change UE4MultiWindowEditor to use a unique build environment by setting
	'BuildEnvironment = TargetBuildEnvironment.Unique;'
in the UE4MultiWindowEditorTarget constructor, or set 
	bOverrideBuildEnvironment = true
to force this setting on.
```

Adding
```cs
DefaultBuildSettings = BuildSettingsVersion.Latest;
```
to both `.Taget.cs` files.

```shell
⎇  master➤  /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/others/UE4MultiWindow/UE4MultiWindow.uproject \
	-TargetType=Editor
# Snip.
Unable to find plugin 'PhysXVehicles' (referenced via UE4MultiWindow.uproject). Install it and try again, or remove it from the required plugin list.
```

That's going to be a problem.
Don't have PhysX anymore.
Disabling that by changing `"Enabled": true` to `"Enabled": false` in `UE4MultiWindow.uproject`.
I don't care if the vehicle breaks.

```shell
⎇  master➤  /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/others/UE4MultiWindow/UE4MultiWindow.uproject \
	-TargetType=Editor
# Snip.
Could not find definition for module 'PhysXVehicles', (referenced via Target -> UE4MultiWindow.Build.cs)
```
Makes sense, if the project enables the plugin then it is not unreasonable to have modules that depend on it.
Commenting out `PhysXVehicles` from `UE4MultiWindow.Build.cs`.
This is going to lead to compiler errors.

```shell
⎇  master➤  /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/others/UE4MultiWindow/UE4MultiWindow.uproject \
	-TargetType=Editor
# Snip.
/media/s1300/unreal_projects/others/UE4MultiWindow/Source/UE4MultiWindow/UE4MultiWindowWheelFront.h(9):
Error: Unable to find parent class type for
	'UUE4MultiWindowWheelFront'
named
	'UVehicleWheel'

/media/s1300/unreal_projects/others/UE4MultiWindow/Source/UE4MultiWindow/UE4MultiWindowWheelRear.h(9):
Error: Unable to find parent class type for
	'UUE4MultiWindowWheelRear'
named
	'UVehicleWheel'

/media/s1300/unreal_projects/others/UE4MultiWindow/Source/UE4MultiWindow/UE4MultiWindowPawn.h(16):
Error: Unable to find parent class type for
	'AUE4MultiWindowPawn'
named
	'AWheeledVehicle'
```

Making the wheels inherit from `UObject` instead of `UVehicleWheel` since that is what `UVehicleWheel` inherits from, and including `"UObject/Object.h"` instead of `VehicleWheel.h`.

Making `AUE4MultiWindowPawn` inherit from `APawn` instead of `AWheeledVehicle`, since that is what `AWheeledVehicle` inherits from. Including `GameFramework/Pawn.h` instead of `WheeledVehicle.h`.

I don't see any overridden virtual member functions I need to make non-override.
Building again.
```shell
⎇  master➤  /media/s1300/unreal_engine/5.4.2/Engine/Build/BatchFiles/Linux/Build.sh \
	Linux \
	Development \
	-Project=/media/s1300/unreal_projects/others/UE4MultiWindow/UE4MultiWindow.uproject \
	-TargetType=Editor
# Snip.
/media/s1300/unreal_projects/others/UE4MultiWindow/Source/UE4MultiWindow/UE4MultiWindowWheelFront.cpp:4:10:
fatal error: 'TireConfig.h' file not found

/media/s1300/unreal_projects/others/UE4MultiWindow/Source/UE4MultiWindow/UE4MultiWindowWheelRear.cpp:4:10:
fatal error: 'TireConfig.h' file not found

/media/s1300/unreal_projects/others/UE4MultiWindow/Source/UE4MultiWindow/UE4MultiWindowPawn.cpp:15:10:
fatal error: 'WheeledVehicleMovementComponent4W.h' file not found

/media/s1300/unreal_projects/others/UE4MultiWindow/Plugins/PlayScene/Source/PlayScene/Public/PlaySceneSlate.h:4:10:
fatal error: 'SCompoundWidget.h' file not found
```

More PhysX Vehicles errors, but also one about `SCompoundWidget`, which is more surprising.

Remove `TireConfig.h` include from both wheels, and empty the constructor.
Remove everything vehicle-related from `UE4MultiWindowsPawm.cpp`.

Build again.
```shell
/media/s1300/unreal_projects/others/UE4MultiWindow/Source/UE4MultiWindow/UE4MultiWindowPawn.cpp:27:10:
fatal error: 'HeadMountedDisplayFunctionLibrary.h' file not found                                                           
/media/s1300/unreal_projects/others/UE4MultiWindow/Plugins/PlayScene/Source/PlayScene/Public/PlaySceneSlate.h:4:10:
fatal error: 'SCompoundWidget.h' file not found
```

Don't need head-mounted display support.
That bit is guarded behind `#if HMD_MODULE_INCLUDED`.
Not sure why that is true if it leads to compiler errors.
According to the [Unreal Engine repository on GitHub](https://github.com/EpicGames/UnrealEngine/blob/release/Engine/Plugins/Runtime/XRBase/Source/XRBase/Public/HeadMountedDisplayFunctionLibrary.h) that file exists and should be includable with that path.
Perhaps missing a module dependency in `.Build.cs`?
I'm just gonna disable everything HMD related by adding ` && false` behind `#if HMD_MODULE_INCLUDED`.
(
Future self:
What I should have done was to change the `PublicDefinitions.Add("HMD_MODULE_INCLUDED=1");` line in `UE4MultiWindow.Build.cs`.
)

Build again.
Got a shit-tonne of errors because `AUE4MultiWindowPawn` uses member functions it used to get from `WheeledVechicle` but no longer does.
Making it inherit from Character instead of Pawn, a bit closer.

Building again.
Errors on things being private in Scene Component.
I recognize this, there are getter functions now.

Build again.
File not found on `SCompoundWidget.h`.
Need to include `Widgets` in the path.

Build again.
File not found on `SlateApplication.h`.
It is now in `Framework/Application`.
See [FSlateApplication @ dev.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Slate/Framework/Application/FSlateApplication).

Build again.
File not found on `SViewport.h`.
Probably need `Widgets`. [Yep](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Slate/Widgets/SViewport).
Same for Tool Tip and Window.

Build again.
Error on `UTextureRenderTarget2D::Resource`.
Is there a getter function for this now? [Yep](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Engine/UTexture/GetResource/1).

Build again.
Linker error:
```
[1/4] Link (lld) libUnrealEditor-UE4MultiWindow.so

ld.lld: error: undefined symbol:
	UPhysicalMaterial::GetPrivateStaticClass()
>>> referenced by PhysicalMaterial.h:105
(Runtime/PhysicsCore/Public/PhysicalMaterials/PhysicalMaterial.h:105)

/media/s1300/unreal_projects/others/UE4MultiWindow/Intermediate/Build/Linux/x64/UnrealEditor/Development/UE4MultiWindow/UE4MultiWindowPawn.cpp.o:

(UPhysicalMaterial* ConstructorHelpersInternal::FindOrLoadObject<UPhysicalMaterial>(FString&, unsigned int))
>>> referenced by PhysicalMaterial.h:105 

(Runtime/PhysicsCore/Public/PhysicalMaterials/PhysicalMaterial.h:105)
>>>               /media/s1300/unreal_projects/others/UE4MultiWindow/Intermediate/Build/Linux/x64/UnrealEditor/Development/UE4MultiWindow/UE4MultiWindowPawn.cpp.o:(UPhysicalMaterial* ConstructorHelpersInternal::FindOrLoadObject<UPhysicalMaterial>(FString&, unsigned int))
```

This is in the engine itself.
A missing module dependency?
[Documentat](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/PhysicsCore/PhysicalMaterials/UPhysicalMaterial) says Physics Core.
To which project module, i.e. which `.Build.cs`, should Physics Core be added?
There is two candidates:
- `UE4MultiWindow.Build.cs`
- `PlayScene.Build.cs`.

The error is from `UE4MultiWindowPawn.cpp.o`, which is part of `UE4MultiWindow`.
Adding the Physics Core module to that module's `.Build.cs`.

Build again.
Success.
Open Unreal Editor.
A lot of warnings.
Several on a vehicle animation blueprint.
One on `/Script/PhysXVehicles`.
Something about Reset VR.

Still, Unreal Editor opens.
There is no vehicle, but who cares?
There is supposed to be a Bring Up Capture Window button in the Main Tool Bar.
I don't see it.
That part of the Unreal Editor UI has been redone since Unreal Engine 4.
Maybe the button-adding code failed?
Can I make it appear some other way?
Where is the code that creates the button?
In the [YouTube Video](https://www.youtube.com/watch?v=8tyCK89ceMM) we see that the tool tip text is "Bring up PlayCapture window".
Searching for that string.
No results.
That is surprising.
Reading some code.

Found that `PlayScene.cpp` seems to setup a keyboard shortcut for opening the window.
No, it is just a command list.
Not bound to a keyboard shortcut, but to the tool bar button.
Which didn't appear.

Enable Editor Preferences > General > Miscellaneous > Developer Tools > Display UI Extension Points to see what extension points we have now.
"Content" seems to be one, testing that instead of the current "Settings".

```shell
[1/3] Compile Module.PlayScene.cpp  
In file included from /media/s1300/unreal_projects/others/UE4MultiWindow/Plugins/PlayScene/Intermediate/Build/Li  
nux/x64/UnrealEditor/Development/PlayScene/Module.PlayScene.cpp:5:                                                 
/media/s1300/unreal_projects/others/UE4MultiWindow/Plugins/PlayScene/Source/PlayScene/Private/PlaySceneCommands.  
cpp:9:107: warning: 'FInputGesture' is deprecated: Use FInputChord instead of FInputGesture Please update your c  
ode to the new API before upgrading to the next release, otherwise your project will no longer compile. [-Wdepre  
cated-declarations]                                                                                                
       UI_COMMAND(OpenPluginWindow, "PlayScene", "Bring up PlayScene window", EUserInterfaceActionType::Button,  
FInputGesture());                                                                                                 
                                                                                                                 
^                                                                                                                 
/media/s1300/unreal_engine/5.4.2/Engine/Source/Runtime/Slate/Public/Framework/Commands/InputChord.h:18:1: note:  
'FInputGesture' has been explicitly marked deprecated here                                                         
UE_DEPRECATED(4.21, "Use FInputChord instead of FInputGesture")                                                    
^                                                                                                                  
/media/s1300/unreal_engine/5.4.2/Engine/Source/Runtime/Core/Public/Misc/CoreMiscDefines.h:274:43: note: expanded  
from macro 'UE_DEPRECATED'                                                                                        
#define UE_DEPRECATED(Version, Message) [[deprecated(Message " Please update your code to the new API before upg  
rading to the next release, otherwise your project will no longer compile.")]]                                     
                                         ^
```

I got a button, and clicking it does give me a window with the scene in.
Unfortunately, the effect of the [[Post Process Volume]] isn't rendered in this view.
Why not?
Limitation of Scene Capture Component?
https://forums.unrealengine.com/t/scene-capture-doesnt-render-post-process/105626 has some hints.

Export the project with Binary Configuration set to Shipping.
```
[2024.07.08-09.47.09:780][471]UATHelper: Packaging (Linux):
Unable to instantiate module 'UnrealEd':
Unable to instantiate UnrealEd module for non-editor targets.

1001[2024.07.08-09.47.09:812][474]PackagingResults: Error:
AutomationTool was unable to run successfully. Exited with code: 8

1001[2024.07.08-09.47.09:812][474]PackagingResults: Error:
Unknown Error
```

So it still tries to use `UnrealEd` even in a packaged project.
Does it need to?
Probably caused by `PlayScene.Build.cs` adding `UnrealEd` to Private Dependency Module Names.
If I remove that than a bunch of code will no longer compile.
The proper solution is to move it to a new editor module.
For now I'm going to hide them all with `#if WITH_EDITOR`.
Is there a similar check we can do in `.Build.cs`?

https://forums.unrealengine.com/t/how-to-detect-if-its-editor-build-in-build-cs/427255 says yes.
Either
```cs
if (Target.Type == TargetRules.TargetType.Editor)
{
	PrivateDependencyModuleNames.Add("UnrealEd");
}
```
or
```cs
if (Target.bBuildEditor)
{
	PrivateDependencyModuleNames.Add("UnrealEd");
}
```

Testing with the latter one first.
Building the editor variant.
Success.
Exporting project.
```
[2024.07.08-09.55.45:044][448]UATHelper:
Packaging (Linux):
Unable to instantiate module 'UnrealEd':
Unable to instantiate UnrealEd module for non-editor targets.
```

Still same error.
Trying the other if expression: `Target.Type == TargetRules.TargetType.Editor`.
Same error.
I see that the error message contains more inform
```
(referenced via Target -> PlayScene.Build.cs -> LevelEditor.Build.cs -> SourceControlWindows.Build.cs -> UnsavedAssetsTracker.Build.cs)
```
Makes sense.
Adding that to the with-editor block of `PlayScene.Build.cs` as well.
Any other module that looks suspiciously editor-related?
No.

Try again.
```
[2024.07.08-10.01.33:795][455]UATHelper: Packaging (Linux): /media/s1300/unreal_projects/others/UE4MultiWindow/Plugins/PlayScene/Source/PlayScene/Private/PlayScene.cpp:6:10: fatal error: 'LevelEditor.h' file not found
[2024.07.08-10.01.33:795][455]UATHelper: Packaging (Linux): #include "LevelEditor.h"
[2024.07.08-10.01.33:795][455]UATHelper: Packaging (Linux):          ^~~~~~~~~~~~~~~
[2024.07.08-10.01.33:795][455]UATHelper: Packaging (Linux): 1 error generated.
1001[2024.07.08-10.01.33:796][455]PackagingResults: Error: 'LevelEditor.h' file not found
```
Got a bit farther.
This is where we need `#if WITH_EDITOR`.

I think it is safe to re-run the packaging from Unreal Editor without closing and recompiling.
The packaging build is done separately.

Try again.
Success.
Run it.
```shell
/media/s1300/unreal_projects/packaged_projects/UE4MultiWindow/Linux
➤ ./UE4MultiWindow.sh -Windowed -ForceRes -ResX=1920 -ResY=1080
```
Works, I get two windows.


So in the end this got us a second window in which a snapshot of the scene is shown.
The actual game viewport is still a separate window.


# Claude AI

Copy-paste from the Claude AI doesn't work, so picking and choosing parts here.

```cpp
class FMySlateApp : public FSlateApplication
{
    // Implementation
};
```

```cpp
class SGameViewport : public SViewport
{
    // Implementation
};
```


```cpp
TSharedRef<SWidget> CreateMainLayout()
{
    return SNew(SVerticalBox)
        + SVerticalBox::Slot()
        .AutoHeight()
        [
            SNew(STextBlock)
            .Text(FText::FromString("Other UI Elements"))
        ]
        + SVerticalBox::Slot()
        .FillHeight(1.0f)
        [
            SNew(SHorizontalBox)
            + SHorizontalBox::Slot()
            .FillWidth(0.7f)
            [
                SNew(SGameViewport)
                // Configure your game viewport here
            ]
            + SHorizontalBox::Slot()
            .FillWidth(0.3f)
            [
                SNew(SVerticalBox)
                // Add other panels or UI elements here
            ]
        ];
}
```


```cpp
FMySlateApp::FMySlateApp()
{
    // Other initialization code

    TSharedRef<SWindow> MainWindow = SNew(SWindow)
        .Title(FText::FromString("My Slate Game"))
        .ClientSize(FVector2D(1280, 720));

    MainWindow->SetContent(CreateMainLayout());

    AddWindow(MainWindow);
}
```

```cpp
class FMyGameViewportClient : public FGameViewportClient
{
    // Implementation
};
```

```cpp
class SGameViewport : public SViewport
{
    // ... other members ...

    TSharedPtr<FMyGameViewportClient> ViewportClient;

    void Construct(const FArguments& InArgs)
    {
        SViewport::Construct(SViewport::FArguments());

        ViewportClient = MakeShared<FMyGameViewportClient>(GetViewportWidget());
        SetViewportInterface(ViewportClient.ToSharedRef());
    }
};
```

```cpp
UWorld* GameWorld = // Get your game world instance
GameWorld->GetGameViewport()->SetViewportClient(ViewportClient.Get());
```

```cpp
TSharedPtr<FSceneViewport> SceneViewport = MakeShared<FSceneViewport>(ViewportClient.Get(), GetViewportWidget());
ViewportClient->SetViewport(SceneViewport.Get());
```

```cpp
GetViewportWidget()->SetViewportInterface(SceneViewport.ToSharedRef());
```

Even copy-pasting just the code is too much since Claude wasn't able to produce working code on the first attempt.
Instead I will make short statements here and what we try.

The first attempt started with `class FMySlateApp : public FSlateApplication`.
This doesn't work because `FSlateApplication` doesn't have any public or protected constructor.
So let's not create a new Slate Application class.

Next it suggests using `FSlateApplication::Get()` to access the existing `FSlateApplication` instance.
Which makes sense.
It gave me a bunch of code, but it makes so many type errors and I'm trying my best to guess what I should use instead but I'm so lost.
I got it to build and run for a bit, but
```
Unhandled Exception: SIGSEGV: invalid attempt to read memory at address 0x0000000000000238

0x00007471ed63cd67
libUnrealEditor-MySlateExper.so
!
TSlateDecl
<
	SExperGameViewport,
	RequiredArgs::T0RequiredArgs
>::operator<<=
(
	SExperGameViewport::FArguments const&
)
[
	Engine/Source/Runtime/SlateCore/Public/Widgets/DeclarativeSyntaxSupport.h:976
]

0x00007471ed63aaf2
libUnrealEditor-MySlateExper.so
!
FExperSlateManager::CreateMainLayout()
[
	MySlateExper/Source/MySlateExper/Private/UI/ExperSlateManager.cpp:58
]
```

`ExperSlateManager.cpp:58` is
```cpp
SAssignNew(GameViewport, SExperGameViewport)
```

I have no idea what I'm doing, which is a sure sign that I'm doing it wrong.

The AI chat bot is good at giving me things to look at, but is useless for producing actual code.
It gave me this summary:

> This setup connects your custom Slate viewport to Unreal's rendering system. The key points are:
> 
> 1. Creating a custom GameViewportClient
> 2. Setting up the SMyGameViewport Slate widget
> 3. Connecting the viewport to the game world
> 4. Creating and setting up the SceneViewport
> 5. Updating the GameEngine and World references
> 6. Handling input in the custom GameViewportClient
> 7. Proper cleanup when the game ends


> You're absolutely right, and I apologize for the confusion in my previous responses. Let's take a step back and clarify the key components and their relationships in this custom Slate viewport setup:
> 
> Key Classes and Objects:
> 
> 1. UGameViewportClient
> 2. SViewport (Slate widget)
> 3. FSceneViewport
> 4. UWorld
> 5. UGameInstance
> 6. APlayerController
> 
> Relationships and Setup:
> 
> 1. UGameViewportClient:
>     - Should know about: SViewport, FSceneViewport, UWorld, UGameInstance
>     - The engine typically instantiates this, but you'll create a custom subclass
> 2. SViewport (Your custom Slate viewport widget):
>     - Should know about: UGameViewportClient, FSceneViewport
>     - You'll instantiate this
> 3. FSceneViewport:
>     - Should know about: UGameViewportClient, SViewport
>     - You'll instantiate this
> 4. UWorld:
>     - Already exists, managed by the engine
>     - Needs to know about your UGameViewportClient
> 5. UGameInstance:
>     - Already exists, managed by the engine
>     - Your UGameViewportClient needs to know about this
> 6. APlayerController:
>     - Already exists, managed by the engine
>     - Interacts with UGameViewportClient for input handling
> 
> Classes to Subclass:
> 
> 1. UGameViewportClient - Create your custom subclass
> 2. SViewport - Create your custom subclass
> 
> Objects to Instantiate:
> 
> 1. Your custom SViewport subclass
> 2. FSceneViewport
> 
> Objects the Engine Instantiates:
> 
> 1. UGameViewportClient (but you'll create your custom subclass)
> 2. UWorld
> 3. UGameInstance
> 4. APlayerController
> 
> Setup Process:
> 
> 1. Create your custom UGameViewportClient subclass
> 2. Create your custom SViewport subclass
> 3. In your game mode or wherever you're setting up the UI:
> 	1. Instantiate your custom SViewport
> 	2. Create the FSceneViewport
> 	3. Create your custom UGameViewportClient (or have the engine create it)
> 	4. Connect these objects to each other and to the existing UWorld, UGameInstance
>
> This setup maintains the separation of concerns while allowing you to customize the viewport behavior. The UGameViewportClient acts as the main coordinator between the game logic and the rendering, while the SViewport and FSceneViewport handle the actual display and input.


# Unreal Editor New Project Wizard

Create a new blank C++ project without starter content named MySlateExper.
Add Slate and Slate Core to Private Dependency Module Name in `MySlateExper.Build.cs`.




# References

- [_UnrealSlateAppTemplate_ by Michael S. Kataev @ github.com 2024](https://github.com/lpestl/UnrealSlateAppTemplate)
- [_Slate UI Framework_ by Epic Games @ dev.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/slate-user-interface-programming-framework-for-unreal-engine)
- [_Making UIs With C++ in Unreal Engine, by Ben UI_ by JetBrains @ youtube.com 2022](https://www.youtube.com/watch?v=1n3oIfI7nBM)
- [_Unreal UIs and C++: Slate_ by ben ui](https://benui.ca/unreal/ui-cpp-slate/)
- [_UE4 Plugin for Scene Capture rendering to separate window_ by Batname @ github.com 2019](https://github.com/Batname/UE4MultiWindow)

Someone using `SceneCapture2D`, `UMaterial`, `FSlateImageBruch`, ans `SImage` to render into a Slate widget.
https://forums.unrealengine.com/t/rendering-only-to-slate-viewport/129914/2

Someone who want to do exactly what I want. No replies.
https://forums.unrealengine.com/t/what-is-the-best-approach-of-having-viewport-drawn-next-to-slate-ui/478548

Some hints for how to create extra viewports in the editor. Not exactly what I need, but may give some hints on what to look for.
https://forums.unrealengine.com/t/how-to-add-a-viewport-to-custom-editor-window/416454

