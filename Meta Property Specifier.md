A Meta Property Specifier is one that only exists in the Blueprint Editor,
not in a [[Packaged Project]].
The Meta Property Specifiers are given as a list to the `Meta` [[Property Specifier]].

Example Meta Property Specifiers are `BlueprintSpawnableComponent` for classes and `DisplayPriority` for [[Property|Properties]].

```cpp
CLASS(Blueprintable, Meta=(BlueprintSpawanbleComponent)
class MYPROJECT_API UMyComponent : public UComponent
{
	GENERATED_BODY()
public:
	UPROPERTY(EditAnywhere, Category = "My Project", Meta = (DisplayPriority=2)
	double ThisGoesBelow;

	UPROPERTY(EditAnywhere, Category = "My Project", Meta = (DisplayPriority=1)
	double ThisGoesAbove;
};
```

