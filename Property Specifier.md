A Property Specifier is a piece of meta-data attached to a [[Property]] using the [[UPROPERTY]] macro.
It is read by Unreal Header Tool and controls the behind-the-scenes C++ code it generates.

Some common Property Specifiers:
- `Blueprint(ReadWrite|ReadOnly)`
- `(Edit|Visible)(DefaultsOnly|InstanceOnly|Anywhere)` [[Property Visibility And Editability]]
- `Category = "MyCategory"`
- `BlueprintAssignable`
	- Used with a [[Delegate]] that should be bindable from a [[Blueprint Class]].

Example:
```cpp
UCLASS()
class MYPROJECT_API UMyClass : public UObject
{
	GENERATED_BODY()
public:
	UPROPERTY(BlueprintReadWrite, EditAnywhere, Category = "MyCategory")
	double MyProperty = 0.0;
};
```


# Meta

There is a special type of Property Specifier called the [[Meta Property Specifier]].
It is a collection of sub-properties that are used by Unreal Editor.
Some examples of Meta sub-properties:

`EditCondition`: A C++ expression, using the other [[Property|Properties]] in the class, that determines if this [[Property]] should be editable or grayed out in [[Details Panel|Details Panels]].
For more details see _Dynamic Edit Condition Based On Other Properties_ in [[Property Visibility And Editability]].

`EditConditionHide`: A C++ expression, using the other [[Property|Properties]] in the class, that determines if this [[Property]] should be visible in [[Details Panel|Details Panels]].

# Other Property Specifiers

- `AssetRegistrySearchable`
	- Makes the [[Property]] searchable and filterable in the [[Content Browser]].

# References

- [_Building Bigger: Changing Your Workflow for Building Worlds instead of Scenes | Unreal Fest 2023_ by Chris Murphy @ dev.epicgames.com/talks-and-demos 2024 UE5.3](https://dev.epicgames.com/community/learning/talks-and-demos/jwlJ/unreal-engine-building-bigger-changing-your-workflow-for-building-worlds-instead-of-scenes-unreal-fest-2023)
