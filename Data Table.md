A Data Table is a container for tabular data, i.e. multiple rows with similar column.
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

```cpp
UCLASS()
class MYMODULE_API AMyActor : public UObject
{
	GENERATED_BODY()
public:
	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "My Actor")
	UDataTable* MyDataTable;
};
```

`FName` is used to index into the Data Table.
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

[[Content Browser]] > right-click > Import To ... > Select a `.csv` or `.json` file.
Select whether to import as a Data Table or something else.
Select the [[Struct]] type to import each item as.
The selected struct should have members that are compatible with the contents of the  imported file.

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
