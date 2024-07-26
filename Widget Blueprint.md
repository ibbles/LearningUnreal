A Widget is a GUI element.
A Widget Blueprint is a Widget designed and implemented as a [[Blueprint Class]] .

A Widget Blueprint contains a collection of sub-Widgets that define the contents of the Widget.
The sub-Widgets are organized in a hierarchy.
Container Widgets can contain other Widgets.
Common container Widgets include Canvas Panel, Horizontal Box, Vertical Box, ...

# Creating And Showing Widgets

Widgets are created with the Create Widget node.
The Create Widget node has an input pin of type Class that is the type of Widget to create.
For a Widget to be rendered it must be added to a Viewport.
Add a Widget to a Viewport with the Add To Viewport member function.
It is common to store a reference to the created Widget so it can be updated later.

Widgets can be created from a Pawn's [[Begin Play Event]].

## Creating A Menu Widget In C++

The following demonstrates how a [[Player Controller]] can create an instance of a Widget Blueprint and add it to the screen.
The example is a menu.
`MenuClass`  is a member variable declared as follows:
```cpp
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "My Player Controller")
TSubclassOf<UUserWidget> MenuClass;
```
The actual widget is defined in Unreal Editor as a Widget Blueprint and Menu Widget Class is assigned to that Widget Blueprint.

```cpp
void AMyPlayerController::PauseGame()
{
	check(MenuWidgetClass != nullptr);
	UUserWidget* Menu = CreateWidget<UUserWidget>(GetWorld(), MenuClass);
	check(Menu != nullptr);
	Menu->AddToViewport();
	SetInputMode(FInputModeUIOnly());
	bShoMouseCursor = true;
}
```

# Live Updates

Widgets may contain dynamic content, i.e. content that changes over time.
One example is the Progress Bar Widget.
One way to update the state of such a Widget is with a Binding.
A Binding is a function that is called every tick.
This has a high performance cost if there are many Bindings.
This cost is especially unnecessary for widgets that don't change often.

An alternative to Bindings is a [[Custom Event]] in the Widget's Event Graph.
The [[Custom Event]] can have a Parameter that is the new state the Widget should display.


# References

- [_C++ For Unreal Engine (Part 2) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ by Nerd's Lesson, A.T. Chamillard @ youtube.com, 2022, UE4.27](https://youtu.be/IYJwU-rB2jA?t=30198)
