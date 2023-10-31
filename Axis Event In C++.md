To get a callback when an [[Axis Event]] fires:

`MyPlayerController.h`:
```cpp
UCLASS()
class MYMODULE_API AMyPlayerController : public APlayerController
{
	GENERATED_BODY()
public:
	virtual void SetupInputComponent() override;
	void MyAxisCallback();
```

`MyPlayerController.cpp`:
```cpp
void AMyPlayerController::SetupInputComponent()
{
	Super::SetInputComponent();
	check(InputComponent != nullptr);
	InputComponent->BindAxis(
		"MyAxisEvent", this, &AMyPlayerController::MyAxisCallback);
}

void AMyPlayerController::MyAxisCallback(float AxisValue)
{
	AMyPawn* Pawn = Cast<AMyPawn>(GetPawn());
	check(Pawn != nullptr);
	Pawn->HandleAxisInput(AxisValue);
```

`MyPawn.h`:
```cpp
UCLASS
class MYMODULE_API AMyPawn : public APawn
{
	GENERATED_BODY()
public:
	void HandleAxisInput(float AxisValue);
};
```

`MyPawn.cpp`:
```cpp
void AMyPawn::HandleAxisInput(float AxisValue)
{
	UE_LOG(LogTemp, Log, TEXT("Gamepad axis has valud %f."), AxisValue);
}
```

`AxisValue` can be negative, either because the gamepad stick was moved left or down, or because a negative scale was set on a key binding in the [[Axis Mappings]].

To do movement based on axis input it is recommended to use the [[Character Movement Component]].
Let's call the Pawn's member function `ForwardInput` instead of `HandleAxisInput` for this example.
Remember to update the [[Player Controller]] correspondingly.
```cpp
void AMyPawn::ForwardInput(float AxisValue)
{
	AddMovementInput(GetActorForwardVector(), AxisValue * MySpeed);
}
```

All the movement inputs that are added can be consumed in `AMyPawn::Tick` by calling `ConsumeMovementInputVector()`.
```cpp
void AMyPawn::Tick(float DeltaTime)
{
	FVector Movement = ConsumeMovementInputCector();
	// TODO: Move according to Movement and DeltaTime.
}
```


See also [[Action Event In C++]].

For input to reach the game during a Play In Editor session, the [[Level Viewport]] must have focus.
Focus it by clicking on it.