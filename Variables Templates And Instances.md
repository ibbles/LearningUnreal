Variables created in the [[Blueprint Actor Editor]] are part of the template for that [[Blueprint Class]].
Instance of that [[Blueprint Class]] get a copy of the variables.
The instance can override the value of the template.
Variables that are overridden have yellow arrow next to them.
Clicking the yellow arrow resets the instance's copy of the variable to the same value as the template.
Difference templates can have different overriding values.
If we change the value in the template then all instances that have not overridden the value get the new value, are updated to use the new value.
Instances that have overridden the value keep their value.

Unreal Editor has bugs related to this updating but I don't remember the details.
Something about two templates and an instance that override the the directly inherited value to the value that the variable has in the template's template.

We can up-apply variables from an instance to the template.
With the instance selected, in the Details panel click the Edit Blueprint > Apply Instance Changes To Blueprint.
I don't know how to selectively up-apply only some of the changed variables.
