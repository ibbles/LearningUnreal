`UPROPERTY` is a macro that decorates a member variable in an `UObject` subclass to make that member variable a [[Property]].
This will mark it for inclusion in Unreal Engine's reflection and serialization systems,
and optionally makes it available in the [[Details Panel]] when an instance of the class is selected.

Example usage:
```cpp
#pragme once
#include "Object/UObject.h"
#include "MyClass.generated.h"

UCLASS()
class MYPROJECT_API UMyClass : public UObject
{
	GENERATED_BODY();
public:
	UPROPERTY()
	double MyData;
};
```

The `UPROPERTY` macro may contain a [[Property Specifier]] to control how that Property is exposed in the Unreal Editor, specifically the [[Details Panel]].

To say that we need read-write access to the [[Property]]:
- `EditDefaultsOnly`
	- Can edit the [[Property]] in a [[Blueprint Class]] in the [[Blueprint Actor Editor]].
	- Cannot edit the [[Property]] when an instance of the class is selected in the [[Level Editor]].
- `EditInstanceOnly`
	- Cannot edit the [[Property]] in a [[Blueprint Class]] in the [[Blueprint Actor Editor]].
	- Can edit the [[Property]] when an instance of the class is selected in the [[Level Editor]].
- `EditAnywhere`
	- Can edit both in the [[Blueprint Actor Editor]] and in the [[Level Editor]].

To say that we need read-only access to the [[Property]]:
- `VisibleDefaultsOnly`
- `VisibleInstanceOnly`
- `VisibleAnyWhere`

A [[Property]] that is included in the [[Details Panel]] appears within a specific [[Category]] within the [[Details Panel]].
We select which category a particular [[Property]] is under with the `Category` [[Property Specifier]].
```cpp
UPROPERTY(Category = "MyCategory")
double MyData;
```


# Meta Data

An additional layer of data is available in the Meta container of a [[Property]].
This is data that is associated with the [[Property]] that are specific to some parts of Unreal Editor.

## `FilePathFilter` for `FFilePath`

Used with a `FFilePath` typed [[Property]].
Detected by `FFilePathStructCustomization` and used to set a file suffix filter on the File Path Picker created by the [[Details Customization]].

For example, to create a [[Property]] that is a path to a `.json` file:
```c++
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Configuration", Meta=(FilePathFilter="json"))
FFilePath ConfigurationFile;
```

Also supports file file type filter syntax.
The equivalent to the above short form would be.
```c++
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Configuration", Meta=(FilePathFilter="json files (*.json)|*.json"))
FFilePath ConfigurationFile;
```

You can also support multiple file suffixes.
```c++
UPROPERTY(
	EditAnywhere, BlueprintReadWrite, Category="Configuration",
	Meta=(FilePathFilter="Configuration files (*.json; *.ini)|*.json; *.ini"))
FFilePath ConfigurationFile;
```


# References

- 1: [_Unreal Engine C++ Complete Guide_ by Tom Looman @ https://www.tomlooman.com/ 2023](https://www.tomlooman.com/unreal-engine-cpp-guide/#C_Syntax_Symbols)

