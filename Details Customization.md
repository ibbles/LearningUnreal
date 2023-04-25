Details customization is the act of configuring the [[Details Panel]] for a particular type.
Details customization is for the entire [[Details Panel]], to configure how a type used as a [[Property]] is shown within the [[Details Panel]], see [[Property Customization]].
Details customization is editor-only, so it must be in an [[Editor Module]], see [[Module]].

The Details panel customization is done in a class that inherit from `IDetailCustomiation`.

```cpp
#include "IDetailCustomization.h"
class MYPROJECTEDITOR_API FMyTypeCustomization : public IDetailCustomization
{
};
```


Classes implementing the `IDetailCustomization` interface should implement two functions:
- `MakeInstance`
	- A static function that returns a `TSharedRef<IDetailCustomization>` to a new instance of the type we are authoring.
- `CustomizeDetails`
	- Where the [[Details Panel]] customization takes place.
	- Is given a `IDetailLayoutBuilder` on which we call functions to customize the [[Details Panel]].


```cpp
#include "IDetailCustomization.h"
class MYPROJECTEDITOR_API FMyTypeCustomization : public IDetailCustomization
{
public:
	static TSharedRef<IDetailCustomization> MakeInstance();

	virtual void CustomizeDetails(IDetailLayoutBuilder& DetailBuilder) override;
};
```

In most cases, `MakeInstance` can be really simple:
```cpp
TSharedRef<IDetailCustomization> FMyTypeCustomization::MakeInstance()
{
	return MakeShareable(new FMyTypeCustomization);
}
```

`CustomizeDetails` can do a lot more.


# Details Panel Pieces

The [[Details Panel]] consists of a number of pieces.
At the top we have the [[Details Panel]] itself.
It consists of two columns, a name column and a value column.
Most properties are shown by a text label with the property's name in the name column and a type-specific view/edit widget in the value column.
Next are the categories, which are named collapsible groups.
A category is a collection of rows.
Most rows have a name part and a value part.
Some rows have a single entry that span the entire row.


# Categories

The [[Details Panel]] is by default populated with categories based on the `Category` [[Property Specifier]] set for the [[Property|Properties]] that the customized type has.
we get a handle to a particular category instance with `IDetailLayoutBuilder::EditCategory(FName CategoryName)`.
Returned is an `IDetailCategoryBuilder`.
If the passed `CategoryName` isn't a category yet then one is created.
The categories are ordered in the [[Details Panel]] in the order in which we call `EditCategory` for them.
Categories from [[Property Specifier|Property Specifiers]] categories we do not edit explicitly are added after the explicitly edited categories.