HUD is short for Heads-Up Display.
It is the 2D elements that are rendered "on the camera lens"-ish, i.e. the UI or User Interface.
Often used to display health bars, ammo counters, mini-maps, and other gameplay related state that the player need to have quick access to.
The `AHUD` class is a manager for putting widgets on the screen.

The HUD is displayed on the player's [[Viewport]].
Elements can be added to and removed from the HUD based on gameplay events and logic.

There are two HUD-capable systems in Unreal Engine:
- The HUD class.
- [[Unreal Motion Graphics - UMG]].

This note is about the HUD class.

The HUD is spawned by each player's [[Player Controller]] as part of the instantiation process.
This doesn't mean that the HUD logic must be in the [[Player Controller]],
you can put it anywhere it makes sense.

I don't know the difference between having a HUD vs adding a [[Widget Blueprint]] to the Viewport.

The HUD class inherits from [[Actor]].

**Create a new HUD** Blueprint by Content Browser > right-click > Blueprint Class > type HUD in the search field > Click Select.
HUD Blueprints are often named with a `HBP_` prefix.

Make a particular HUD class the **active** one by selecting it in [[Game Mode]] > Details panel > Classes > HUD.
Make sure the [[Game Mode]] is selected in either the [[World Settings]] or the [[Project Settings]].

The HUD can be built using [[Unreal Motion Graphics - UMG]], UMG, something similar you build yourself, or using a plugin.

The HUD is owned by the [[Player Controller]], and if we have a reference to the [[Player Controller]] then we can get the HUD with the Get HUD node.

A HUD is a [[Blueprint Class]] inheriting from [[Actor]] and therefor has a Components list, a My Blueprint panel, a Viewport, and an [[Event Graph]].

I'm not sure how to best communicate new data to the HUD.
One one is to have the [[Player State]] notify the HUD about any changes in the [[Player State]].
I thing you can get a hold of the HUB object from a [[Player State]] member function via the [[Player Controller]].

HUD has a [[Blueprint Event]] named **Event Receive Draw HUD** which is given the width and the height of the screen.
The size is specified with integers so I assume they are in pixels, or [[DPI - Dots Per Inch, Scaling]] scaled pixels.
The screen size parameters are named `Sizex X` and `Size Y`.

Drawing elements on the HUD is done with various **Draw** nodes.
The Draw nodes are given Screen X and Screen Y coordinates.
I believe those are in pixels, with (0, 0) being the upper-left corner and (`Size X - 1`, `Size Y - 1`) being the lower-right corner.

A HUD can draw a [[Texture]] with the **Draw Texture** node.
The position where the [[Texture]] is drawn specified with floats.
The width (Screen W) and height (Screen H) of the rendered texture is specified in pixels.
Usually set to the size of the source texture, seen in the pop-up that is displayed when hovering over the [[Texture]] [[Asset]] in the [[Content Browser]].

A crosshair can be created by drawing a texture to the screen at
- `Screen X` = `Screen Size X * POS_X - Texture Size X * 0.5`
- `Screen Y` = `Screen Size Y * POS_Y - Texture Size Y * 0.5`
where `POS_X` and `POS_Y` are often 0.5, i.e. middle of the screen.
By saving the (`Screen Size X * POS_X`,  `Screen Size Y * POS_Y`) coordinate we can use that later to do [[Line Trace]].
Pass the screen coordinate of the crosshair to Player Controller > Deproject Screen To World.


# C++

The HUD class can be implemented in C++.
Inherit from the `AHUD` class.
Override the Draw HUD member function to do any custom drawing.

`MyHUD.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "GameFramework/HUD.h"

#include "MyHUD.generated.h"

UCLASS()
class MYMODULE_API AMyHUD : public AHUD
{
	GENERATED_BODY()

public:
	//~ Begin AHUD interface.
	virtual void DrawHUD() override;
	//~ End AHUD interface.
};
```

## Draw Text In C++

To draw text we need some text to draw, a location to draw it at, and a font to draw it with.
For this example we're drawing the score.

`MyHUD.h`:
```c++
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "GameFramework/HUD.h"

#include "MyHUD.generated.h"

class UFont;

UCLASS()
class MYMODULE_API AMyHUD : public AHUD
{
	GENERATED_BODY()

public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My HUD")
	UFont* Font;

public: // Member functions.
	void GiveScore(int32 Amount);

public: // Member function overrides.
	//~ Begin AHUD interface.
	virtual void DrawHUD() override;
	//~ End AHUD interface.

private:
	int32 Score {0};
};
```

`MyHUD.cpp`:
```cpp
#include "MyHUD.h"

// Unreal Engine includes.
#include "Engine/Font.h"
#include "Math/Color.h"

void AMyHUD::DrawHUD()
{
	Super::DrawHUD();

	const float LeftOffset {50.0f};
	const float TopOffset {100.0f};
	// TODO Find out how to do text formatting.
	DrawText(
		LOCTEXT("score", "Score: ") + FString::FromInt(Score),
		FLinearColor::White, LeftOffset, TopOffset, Font);
}
```

To set the font, create a [[Blueprint Class]] inheriting from `AMyHUD` and set the variable in the [[Blueprint Class Editor]].

## Get The C++ HUD

We can use `UGAmeplayStatics` to get a pointer to the HUD object for a particular player.
It is often better to use an [[Event]] instead of directly accessing the HUD.

```cpp
void AMyPawn::StarCollected()
{
	APlayerController* Controller = GameplayStatics::GetPlayerController(this, 0);
	AMyHUD* Hud = Controller->GetHUD<AMyHUD>();
	Hud->GiveScore(1);
}
```

- [_C++ For Unreal Engine (Part 2) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_](https://youtu.be/IYJwU-rB2jA?t=11785)


# References
- [Creating a Block-Based Game - Adding the PlayerCharacter, Crosshair, and GameMode @ learn.unrealengine.com](https://learn.unrealengine.com/course/3770925/module/7308627)
- [_C++ For Unreal Engine (Part 2) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ by Nerd's Lesson, A.T. Chamillard @ youtube.com, 2022, UE4.27](https://youtu.be/IYJwU-rB2jA?t=11785)
