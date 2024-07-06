Slate is a C++ framework for building user interfaces.
Contains a bunch of widgets: windows, buttons, sliders, images, progress bars. Stuff like that.
Contains a bunch of containers for other widgets: overlay, vertical box, horizontal box, border. Stuff like that.
Also handles user input such as keyboard, mouse, joysticks, gamepads, touch.
Used by both Unreal Editor and in-game interfaces.
[[Unreal Motion Graphics - UMG]] is based on Slate.
Each UMG widget type has a corresponding Slate widget type.
The UMG widget instance has a pointer to a Slate widget instance.
The UMG type names start with `U`, while the Slate type names start with `S`.

Slate is implemented in two [[Module]]s:
- Slate Core: Low-level functionality.
- Slate: Library of widgets.

They can be found in Engine > Source > Runtime.

Add them to the Private Dependency Module Names list in your `.Build.cs` file for the [[Module]] that uses Slate:
```cs
PrivateDependencyModuleNames.AddRange(new string[]
{
    "Slate", "SlateCore"
});
```

Slate uses a lot of macros. They are described further in the rest of this note.
- `SNew`: Factory function to create a new instance of a widget.
- `SLATE_BEGIN_ARGS`: Used in widget class definitions to begin the argument list.
	- Declares a nested struct named `FArguments`.
- `SLATE_END_ARGS`: Used in widget class definitions to end the argument list.
- `SLATE_ARGUMENT`: Declare an argument that can be configured on the return value from `SNew`.
	- Expands to a member variable inside the `FArguments` struct along with a set function for it.
- `SLATE_ATTRIBUTE`: Declare a configurable attribute on the widget.
	- Creates functions that can be called on the return value from `SNew`.
	- Should be placed between `SLATE_BEGIN_ARGS` and `SLATE_END_ARGS`.
- `SLATE_EVENT`: Declare a bindable event on the widget.
	- Creates an event that callbacks that can be registered with on the return value from `SNew`.
	- Should be placed between `SLATE_BEGIN_ARGS` and `SLATE_END_ARGS`.

The Slate widgets are not [[UObject]]s, which means that they are not considered by the garbage collector.
If you need a pointer to a [[UObject]] then that pointer should be a [[Weak Pointer|TWeakObjectPtr]].


# Building UI

Slate interfaces are created using a declarative syntax.
Entire interface blocks, i.e. hierarchies of widgets, are created and configured with a single C++ expression.
The expression is recursive, meaning each level contains slots in which we place nested widgets using the same expression syntax.
The contents of a slot is passed to `operator[]`.
There is a factory function named `SNew` that should be used when creating a new widget instance.
The sole parameter to `SNew` is the type to instantiate.
Here is an example that creates a button on which we put a text block.
```cpp
SNew(SButton)
[
	SNew(STextBlock)
];
```
This is how compositing is done in Slate.
The text block lives within a slot in the button.

We can chain member function calls on the return value from `SNew` to configure the created object.
These member functions either set an attribute or bind a callback to an event.
Attributes are listed in the widget class definition with `SLATE_ATTRIBUTE`.
Events are listed in the widget class definition with `SLATE_EVENT`.
More on that in _Custom Widget_.

Extending the button-with-text example with a callback, some text, and a tool-tip.
```cpp
SNew(SButton)
.OnClicked(/* Some callback function */)
[
	SNew(STextBlock)
	.Text(LOCTEXT("ButtonText", "Click Me!"))
	.ToolTipText(LOCTEXT("ButtonToolTip", "Click this button."))
]
```


# Slot

A slot is a thing in which we can place a widget.
A widget may contain slots, which is how we build hierarchies of widgets.
Containers use slots to control the layout of the children.
A Compound Widget has a slot named Child Slot which is where we put the contents of a Compound Widget.
Some widgets, such as Overlay and Horizontal and Vertical Box, has a variable number of slots.
Add a new slot with the + operator, with the widget builder as the left hand side and a widget-specific slot type as the right hand side.
An example that puts three text blocks next to each other within a horizontal box.
```cpp
#define LOCTEXT_NAMESPACE "My Game"

SNew(SHorizontalBox)
+ SHorizontalBox::Slot()
[
	SNew(STextBlock)
	.Text(LOCTEXT("Text1", "First Slot"))
]
+ SHorizontalBox::Slot()
[
	SNew(STextBlock)
	.Text(LOCTEXT("Text2", "Second Slot"))
]
+ SHorizontalBox::Slot()
[
	SNew(STextBlock)
	.Text(LOCTEXT("Text3", "Third Slot"))
]

#undef LOCTEXT_NAMESPACE
```

# Custom Widget

When we create a custom widget we have a few options for what base Slate class to inherit from:
- Leaf Widget: When we have no children and will do our own rendering.
- Panel: When we are a container with a variable number of child widgets.
- Compound Widget: When we control our own collection of child widgets.
	- This is the most commonly used base class.

Most custom widgets are created as compounds of other widgets.
To create a new type of widget inherit from `SCompoundWidget`.
Slate widget type names always start with an `S`.
When defining the widget type we list all the inputs, i.e. arguments, between `SLATE_BEGIN_ARGS` and `SLATE_END_ARGS`.
These inputs can be values such as strings or integers, or callbacks to be bound to events.
List them using the macros
- `SLATE_ARGUMENT`
- `SLATE_ATTRIBUTE`
- `SLATE_EVENT`

These input arguments are passed in an `FArguments` struct to the widget's `Construct` member function.
They become set-functions on the return value from `SNew` when an instance of our custom widget type is being created.
So if we add
```cpp
SLATE_ATTRIBUTE(FText, LabelText)
```
between `SLATE_BEGIN_ARGS` and `SLATE_END_ARGS` then a user will be able to do
```cpp
SNew(SMyWidget)
.LabelText(TEXT("My Label Text"))
```
which will set a member named `_LabelText` in the `FArguments` type.

The `Construct` function is responsible for creating and configuring the child widgets,
as specified by the arguments in the given `FArguments`.
The Compound Widget base class provide a Child Slot member variable which acts as the root for the subtree of widgets we create,
it is where we put the interior of our custom widget.

This example create a custom widget type that is a button with a text label and a tool-tip.

`STextButton.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "Widgets/SCompoundWidget.h"

class STextButton : public SCompoundWidget
{
public:
	SLATE_BEGIN_ARGS(STextButton)
	{}

	SLATE_ATTRIBUTE(FText, LabelText)
	SLATE_ATTRIBUTE(FText, ToolTipText)
	SLATE_EVENT(FOnClicked, OnClicked)

	SLATE_END_ARGS()

	void Construct(const FArguments& InArgs);
}
```

When client code construct an instance of `STextButton` the `Construct` function will eventually be called.
The `FArguments` type passed to `Construct` is specific to this particular widget type and contains members for the attributes and events that was declared with `SLATE_ATTRIBUTE` and `SLATE_EVENT`.
The responsibility of `Construct` is to create the child widgets and configure them according to the given `FArguments`.
During configuration, i.e. the code that calls `SNew(STextButton)`, the `FArguments` object is being configured with member function calls on the return value from `SNew`.
So `SNew` doesn't crate and return an `STextButton`, it creates and returns an `STetButton::FArguments`.
All member functions on `FArguments` returns itself, so the member functions can be chained in a single expression.
This is called a [fluent interface](https://en.wikipedia.org/wiki/Fluent_interface), and is also an example of a [builder pattern](https://en.wikipedia.org/wiki/Builder_pattern).
Then, at the end of the expression or some time later (don't know which), the `FArguments` object is passed to `Construct` and the actual widget is created.

`SCompoundWidget` has a member variable named `ChildSlot` of a type the ultimately inherits from `FSlotBase`, which means that it has an `operator[]` to which we can pass nested widgets created with `SNew`.

Here is how `STextButton::Construct` can be implemented.

`STextButton.cpp`:
```cpp
#include "STextButton.h"

void STextButton::Construct(const FArguments& InArgs)
{
	// ChildSlot is provided by the SCompoundWidget base class
	// and is the slot where we put our child widgets.
	ChildSlot
	// operator[] is used to put a widget into a slot.
	[
		// Use SNew to create a new widget instance in a slot.
		SNew(SButton)
		// Use member functions to set attributes and bind to events.
		// Here we forward the OnClicked event callback from our
		// arguments into the buttons arguments.
		.OnClicked(InArgs._OnClicked)
		// Again, operator[] is used to created nested widgets.
		// Here we nest into the button, which is nested inside
		// our custom text button.
		[
			// Again, SNew is used to create new widget instances.
			SNew(STextBlock)
			// Again, forward from our arguments in the child widget's
			// arguments.
			.Text(InArgs._LabelText)
			.ToolTipText(InArgs._ToolTipText)
		]
	];
}
```


# Adding Slate Widget To Viewport

In this example we add a menu overlay to a [[HUD]].
Then intention is that the game will start with the menu open and the player can open and close the menu at will.

In the [[HUD]] class add a member of type `TSharedPtr<SMyMenu>`,
where `SMyMenu` is some custom widget whose details we don't care about here.
See _Custom Widget_ for details on how to create a widget type.

It is sometimes useful to put the widget inside a container.
Then we can show and hide the container to show and hide the menu.
(
Why not show and hide the menu widget directly?
)

The widget can, for example, be created at Begin Play.

`MyHUD.h`:
```cpp
#pragma once

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "GameFramework/HUD.h"

class SMyMenu;
class SWidget;

UCLASS()
class MYGAME_API AMyHUD : public AHUD
{
	GENERATED_BODY()

protected:
	TSharedPtr<SMyMenu> MenuWidget;
	TSharedPtr<SWidget> MenuContainer;

	//~ Begin UActor interface.
	virtual void BeginPlay() override;
	//~ End UActor interface.
};
```

I'm not sure what the best way to add a widget to the screen in C++ is.
With [[Unreal Motion Graphics - UMG]] we would call the Add To Viewport member function.
That is a member of User Widget, which is an UMG class, so not available here.
[_Make UI With C++: How to use Slate in Unreal Engine_ @ youtube.com](https://www.youtube.com/watch?v=jeK6DPB5weA) suggests we get the [[Viewport]] from the global engine pointer.
I don't think this is the right way (How do we know we add it to the correct viewport?) but I currently don't know of a better way.
`UUserWidget::AddToViewport` uses `UGameViewportSubsystem::AddWidget`.
Can we do the same?
[_Making UIs With C++ in Unreal Engine, by Ben UI_ @ youtube.com](https://www.youtube.com/watch?v=1n3oIfI7nBM&t=603s) suggest we get the [[Game Viewport]] from the [[World]].
That seems safer.

What follows is the variant that goes through the global engine pointer.
It uses `SWeakWidget` as the type for Menu Container.
`SWeakWidget` is a weak pointer to a widget, i.e. something else must keep it alive.
We give `SWeakWidget` a child widget with the Possibly Null Content member function.

`MyHUD.cpp`:
```cpp
#include "MyHUD.h"

// My Game inclues.
#include "MyMenu.h" // Menu implementation not included in this example.

// Unreal Engine includes.
#include "Engine/Engine.h"
#include "Widgets/SWeakWidget.h"

void AMyHUD::BeginPlay()
{
	Super::BeginPlay();

	if (GEngine == nullptr)
		return;
	if (GEngine->GameViewport == nullptr)
		return;

	MenuWidget = SNew(SMyMenu);
	GEngine->GameViewport->AddViewportWidgetContent(
		SAssignNew(MenuContainer, SWeakWidget)
			.PossiblyNullContent(MenuWidget.ToSharedRef()));
}
```

Variant that uses [[World]] instead of `GEngine` to find the viewport client.
```cpp
void AMyHUD::BeginPlay()
{
	Super::BeginPlay();

	MenuWidget = SNew(SMyMenu);
	UGameViewportClient* ViewportClient
		= GetWorld()->GetGameViewport();
	ViewportClient->AddViewportWidgetContent(
		MenuWidget.ToSharedRef());
}
```

If the [[Game Mode]] current in use have My HUD as the registered HUD class then an instance of `AMyHUD` will be created at the start of the game and Begin Play will be called, which in turn creates an instance of the My Menu widget and adds it to the game [[Viewport]].

Menus are often interacted with using the mouse, so we may want to show the mouse cursor.
We often want to disable game input while a menu is open, so that moving the mouse cursor between menu items doesn't also move the camera around.

```cpp
void AMyHUD::BeginPlay()
{
	// Widget creation code here.

	if (PlayerOwner != nullptr)
	{
		PlayerOwner->bShowMouseCursor = true;
		PlayerOwner->SetInputMode(FInputModeUIOnly());
	}
}
```

# Removing Slate Widget From Viewport

In _Adding Slate Widget To Viewport_ we added a menu container to the game [[Viewport]] and stored a pointer to it in Menu Container.
To remove it again, use Remove Viewport Widget Content.
```cpp
void AMyHUD::RemoveMenu()
{
	if (GEngine == nullptr)
		return;
	if (GEngine->GameViewport == nullptr)
		return;
	if (!MenuContainer.IsValid())
		return;

	GEngine->GameViewport->RemoveViewportWidgetContent(
		MenuContainer.ToSharedRef());
}
```

If the widget we are removing had taken input ownership then we should give it back to the game.

```cpp
void AMyHUD::RemoveMenu()
{
	// Widget removal code here.

	if (PlayerOwner != nullptr)
	{
		PlayerOwner->bShowMouseCursor = false;
		PlayerOwner->SetInputMode(FInputModeGameOnly());
	}
}
```


# User Input

## Local Widget Callback

A local widget callback is one that the widget handles itself.
It is not listed among the Slate arguments.

We need a member function that should be run when the button is clicked.
It should take no arguments and return `FReply`.
`FReply` tells the UI framework whether we handled the click or not.
If we did then other callbacks won't get the callback.
If we didn't then the next callback is called.
```cpp
FReply OnButtonClicked() const;
```

To add the callback to a button being created use the On Clicked function.
Pass the `this` pointer and a pointer to the callback function.
```cpp
SNew(SButton)
.OnClicked(this, &UMyWidget::OnButtonClicked)
```

The On Button Clicked function should do some work and then return either `FReply::Handled()` or `FReply::Unhandled()`.


## Mouse

## Keyboard

An `SCompoundWidget` can signal that it can be given keyboard focus by overriding `SupportsKeyboardFocus` to return true and setting `bCanSupportFocus` to true in `Construct`.
I don't know if or why both are required.

`SMyWidget.h`:
```cpp
#pragma once

// Unreal Engine includes.
#incldue "CoreMinimal.h"
#include "Widgets/SCompoundWidget.h"

class SMyWidget : public SCompoundWidget
{
public:
	SLATE_BEGIN_ARGS(SMyWidget)
	{
	}

	void Construct(const FARguments& InArgs);

	//~ Begin SCompoundWidget interface.
	virtual bool SupportsKeyboardFocus() const override
	{
		return true;
	}
	//~ End SCompoundWidget interface.
}
```

`SMyWidget.cpp`:
```cpp
#include "SMyWidget.h"

void SMyWidget::Construct()
{
	bCanSupportFocus = true;
}
```


# Slate Reflector

Unreal Editor has Top Menu Bar > Window > Developer Tools > Widget Reflector (In Unreal Engine 4, may have moved in 5.).
The Widget Reflector is a tool that visualizes how the Unreal Editor interface has been composed.
It lists all the Slate widgets and how they are nested inside of each other.
The Widget Reflector is a separate window.
At the top is a button labeled Pick Live Widget.
Click that and move the mouse cursor over the Unreal Editor UI.
A tree view is displayed in the Widget Reflector window showing the type of widget he mouse cursor is current on and all parent widgets.
Hit Esc to freeze the selection, stopping mouse cursor widget selection and you can inspect the widget tree in detail in the Widget Reflector window.
Each row in the Widget Reflector tree view contains a link to the C++ code where that widget was created.


# References

- [_Slate Architecture_ by Epic Games @ dev.epicgames.com/documentation](https://dev.epicgames.com/documentation/en-us/unreal-engine/understanding-the-slate-ui-architecture-in-unreal-engine)
- [_7 Slate UI Framework & Widget Reflector_ by Max Preussner, Söderberg M @ youtube.com 2021](https://www.youtube.com/watch?v=je3Zid_OmGg)
- [_Make UI With C++: How to use Slate in Unreal Engine_ by reubs @ youtube.com 2020](https://www.youtube.com/watch?v=jeK6DPB5weA)
- [_Making UIs With C++ in Unreal Engine, by Ben UI_ by Ben UI, JetBrains @ youtube.com 2022](https://www.youtube.com/watch?v=1n3oIfI7nBM&t=603s)