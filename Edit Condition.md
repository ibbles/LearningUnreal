Edit Condition is one of the sub-specifiers of the `Meta` [[Property Specifier]].
Its value is a C++ expression that when evaluates to false disables the Property in the Details Panel.
So its a way to make it clear that a setting has no effect, or is otherwise unavailable, during certain circumstances.
There are limitations on what can go into the expression, but I'm not sure what those limitations are.

```cpp
UPROPERTY(Meta = (EditCondition = "bLightEnabled && bLightColored"))
FColor LightColor;

UPROPERTY(EditAnywhere, Category = "Overrides", Meta = (PinHiddenByDefault, InlineEditConditionTogle))
bool bLightEnabled;

UPROPERTY(EditAnywhere, Category = "Overrides", Meta = (PinHiddenByDefault, InlineEditConditionTogle))
bool bLightColored;
```
