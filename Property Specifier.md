A Property Specifier is a piece of meta-data attached to a [[Property]] using the [[UPROPERTY]] macro.
It is read by Unreal Header Tool and controls the behind-the-scenes C++ code it generates.

Some common Property Specifiers:
- `Blueprint(ReadWrite|ReadOnly)`
- `(Edit|Visible)(DefaultsOnly|InstanceOnly|Anywhere)` [[Property Visibility And Editability]]
- `Category = "MyCategory"`

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

`EditCondition`: A C++ expression, using the other [[Property|Properties]] in the class, that determines if this [[Property]] should be editable or grayed out in [[Details Panel|Details Panels]].

`EditConditionHide`: A C++ expression, using the other [[Property|Properties]] in the class, that determines if this [Property]] should be visible in [[Details Panel|Details Panels]].
