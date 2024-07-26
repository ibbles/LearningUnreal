A Data Table is a container for tabular data, i.e. multiple rows with similar columns.
Suitable when working with large amounts of simple data,
not great for nested or hierarchical data.
An alternative to using a Data Table is a [[Data Asset]].

A Data Table is a type of [[Asset]] that lives in the [[Content Browser]].
Can be created in Unreal Editor or imported from `.csv` or `.json`.
Create with Content Browser > right-click > Miscellaneous > Data Table.
Select a [[Struct]] to define the contents of each row of the Data Table.
If using a C++ struct then it must inherit from `FTableRow`.

The Data Table editor contains a column for each struct member.

New rows are added to the Data Table by clicking the Add button in the tool bar.
Data is entered in the Row Editor panel, which works just like a [[Details Panel]].

Existing rows are also edited with the Row Editor panel.
Click a row to select it.

An exception is the row name, the first column.
It is not edited from the Row Editor.
It is instead edited by double-clicking it in the Data Table panel.

The row name is used for look-ups into the Data Table.
That is, a Data Table is a `FName → FMyStruct` map.
It is common to use an [[Enum]] to identify rows.
That is, we associate some extra data, the [[Struct]] members, with each enum literal.
The Data Table becomes a `EMyEnum → FMyStruct` map.
We must do the `EMyEnum` → `FName` conversion ourselves, see _Blueprint_ and _C++_ below.

# Blueprint

There is a [[Blueprint Class]] named Data Actor Table.
(
I think this is part of the tutorial, i.e. just an example.
)
Has a [[Blueprint Function]] named Get Data Table Row.
Select a Data Table [[Asset]] to read from and the name of the row to read.
We can convert an [[Enum]] variable to a row name  by converting it first to a String and then a Name,
this process will remove the name of the Enum type itself.
The Out Row pin can be passed to a Break node to access the individual [[Struct]] members.


# C++

Access Data Tables by having a `UDataTable*` member in a [[UClass]].
This example uses a table that has rows of type `FMyStruct`.

```cpp
UCLASS()
class MYMODULE_API AMyActor : public UObject
{
	GENERATED_BODY()

public:
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "My Actor")
	UDataTable* MyDataTable;

	void UseDataTable(FName RowName);
};
```

The My Data Table member should be set in a [[Blueprint Class]] subclass with the [[Blueprint Class Editor]] or on an instance of My Actor from the [[Details Panel]] to a Data Table [[Asset]].

`FName` is used to index into the Data Table.

```cpp
void AMyActor::UseDataTable()
{
	FMyStruct* Row = MyDataTable->FindRow<FMyStruct>(RowName, TEXT(""));
}
```

The second argument is a context string that I don't know what it is for.

Here is an example where an [[Enum]] is used to identify the row to read.
```cpp
void AMyActor::MyFunction()
{
	const FName RowName = FName(UEnum::GetDisplayValueAsText(MyEnum).ToString());
	FMyStruct* Row = MyDataTable->FindRow<FMyStruct>(RowName, TEXT(""));
}
```
The string parameter is used to generate warning messages.

# Import

When importing a CSV file it is important that the first row contains the names of the columns and that the first column is named Name.
[[Content Browser]] > right-click > Import To ... > Select a `.csv` or `.json` file.
Select whether to import as a Data Table or something else.
Select the [[Struct]] type to import each item, i.e. row, as.
The selected struct should have members that are compatible with the contents of the imported file.

If the struct is a C++ struct then it should follow the following setup, with one [[Property]] for each column of the CSV file except for Name.
`MyStruct.h`:
```cpp
#pragma once.

// Unreal Engine includes.
#include "CoreMinimal.h"
#include "Engine/DataTable.h"

#include "MyStruct.generated.h"

USTRUCT(BlueprintType)
struct MYMODULE_API FMyStruct : public FTableBaseRow
{
	GENERATED_USTRUCT_BODY() // Can we use GENERATED_BODY instead?

public:
	FMyStruct() {};

	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My Struct")
	double SomeData {1.0};

	UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "My Struct")
	int32 OtherData {1};
};
```

If the [[Struct]] type contains an [[Enum]] member and the input file contains an invalid enum literal then it will be set to the first element of the 
[[Enum]].


# Export

A Data Table is exported to either CSV or JSon by right-click and select one of the Export As entries in the context menu.
From C++ we can use `UDataTable::GetTableAsCSV` or `GetTableAsJSON`.
```cpp
FFileHelper::SaveStringToFile(MyDataTable->GetTableAsCSV, TEXT("MyDataTable.csv"));
```

The first column is always the name of the row.
The first column is sometimes named `---`, you can rename it to `Name` if you want.


# References

- [_Improving C++ Workflows Using Data_ by Epic Games @ dev.epicgames.com 2022](https://dev.epicgames.com/community/learning/courses/Xp/unreal-engine-improving-c-workflows-using-data/pY1/unreal-engine-project-overview-and-creating-structs)
- [_C++ For Unreal Engine (Part 2) | Learn C++ For Unreal Engine | C++ Tutorial For Unreal Engine_ by Nerd's Lesson, A.T. Chamillard @ youtube.com, 2022, UE4.27](https://youtu.be/IYJwU-rB2jA?t=17385)
