[[Input Event]] are triggered by [[Input Bindings]] set up in the [[Project Settings]].
During runtime we can listen to those events in a C++ [[Player Controller]].
To listen to an input event we need to register, or bind, the event in our [[Player Controller]] , which is done in the `SetupInputComponent` function.
We bind the event to a callback function.
When the event is triggered then the callback function is called.

All [[Player Controller]] instances have a `UInputComponent` member named `InputComponent`.
Call `BindAction` on that [[Input Component]] for every action that the [[Player Controller]] should respond to.

`MyPlayerController.h`:
```cpp
UCLASS()
class MYMODULE_API AMyPlayerController : public APlayerController
{
	GENERATED_BODY()
public:
	virtual void SetupInputComponent() override;
	void MyActionCallback();
};
```

`MyPlayerController.cpp`:
```cpp
void AMyPlayerController::SetupInputComponent()
{
	Super::SetupInputComponent();
	check(InputComponent != nullptr);
	InputComponent->BindAction(
		"MyActionEvent", EInputEvent::IE_Pressed,
		this, &AMyPlayerController::MyActionCallback);
}

void AMyPlayerController::MyActionCallback()
{
	UE_LOG(LogTemp, Log, TEXT("The action was triggered.");
}
```