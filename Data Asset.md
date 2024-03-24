A Data Asset is an asset type that contain arbitrary structured data.
Works well for hierarchical data.
Supports containers such as [[Array]] and [[Map]].
Suitable for smaller, or even single, pieces of information.
An alternative to a Data Asset is a [[Data Table]], which is made for large sets of simpler data.

Create a new Data Asset with [[Content Browser]] > right-click > Miscellaneous > Data Asset.
Select a `UDataAsset` subclass to base the new Data Asset on.
The `UDataAsset` subclass can be made either in C++ or as a [[Blueprint Class]].
Blueprint Data Asset typically inherit from Primary Data Asset.
It is common to have a C++ [[Struct]] define the data members and then the Blueprint type inheriting from Primary Data Asset has a variable being that struct type.
Then the members of the [[Struct]] will show up in the Data Asset editor when editing a Data Asset instance of the Blueprint class.
We can make the variable in the Blueprint an [[Array]] instead of a single variable to make it possible to store multiple items within a single Data Asset.

Data Assets are populated using the Unreal Editor using the Data Asset editor.
The Data Asset editor is basically just a [[Details Panel]] in a window with a save button.

Example usage:
```cpp
UCLASS()
class MYMODULE_API UMyDataAsset : public UDataAsset
{
	GENERATED_BODY()
public:
	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My Data Asset")
	int32 MyData;
};


UCLASS()
class MYMODULE_API UMyObject : public UObject
{
	GENERATED_BODY()
public:
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "My Object")
	UMyDataAsset* MyDataAsset;
};


void UMyObject::MyFunction()
{
	MyDataAsset->MyData;
}
```

It is possible to copy data from a Data Table row into a Data Asset,
if the data types match.

# Creating A Blueprint Data Asset

Create a [[Blueprint Class]] inheriting from Primary Data Asset.
The [[Blueprint Variable]]s of this [[Blueprint Class]] defines the contents of the Data Asset.
Create a new Data Asset, selecting the [[Blueprint Class]] as the Data Asset Instance.
Open the Data Asset Editor to modify the data held by the Data Asset.

# References

- [_Improving C++ Workflows Using Data_ by Epic Games @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/courses/Xp/unreal-engine-improving-c-workflows-using-data/pY1/unreal-engine-project-overview-and-creating-structs)
