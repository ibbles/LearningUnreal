See also [[UPROPERTY]].

- `Blueprint(ReadWrite|ReadOnly)`
- `(Edit|Visible)(DefaultsOnly|InstanceOnly|Anywhere)`
- `Category = "MyCategory"`


`EditCondition`: A C++ expression, using the other UProperties in the class, that determines if this UProperty should be editable or grayed out in Details Panels.

`EditConditionHide`: A C++ expression, using the other UProperties in the class, that determines if this UProperty should be visible in Details Panels.
