# Editing Component Properties

Find the SCS Node for the Component to edit.
Get the [[Template Component]] from the SCS Node.
Modify the [[Template Component]] as-if it was any regular [[Actor Component]].
This kinda sorta works.
In some cases.
For some things.
There are a few more steps one has to take.

Call `PostEditChangeChainProperty` to propagate changes from the  [[Template Component]], a.k.a. [[Archetype]], to all [[Archetype Instance]]s.
This is not always enough.
Sometimes our C++ code must explicitly find and modify the [[Archetype Instance]]s using `GetArchetypeInstances`.

For the cases where `PostEditChangeChainProperty` work, we must first call `PreEditChange(FEditPropertyChain&)` to tell the system that we are about to modify that propert.
Some `UObject` classes hide that inherited overload with other virtual functions with the same name, so to call it we sometimes have to cast our object pointer to `UObject*`.
There is also an `PreEditChange(FProperty*)` overload, but that doesn't set the `RF_Transactional` flag on the object and its [[Archetype Instance]]s so some objects might not be recorded in the transaction buffer.

`PostEditChangeChainProperty` relies on delta serialization to work, which means that it can only update [[Archetype Instance]] with changes to a [[Property]] that has a type that supports delta serialization.
Arrays do not support array serialization.
This means that `PostEditChangeChainProperty` will not propagate changes to array a [[Property]] to the [[Archetype Instance]]s.
Instead we must do this manually, using `GetArchetypeInstances` to find the [[Archetype Instance]]s.
When doing the update, it is important to change the [[Archetype]] first.
This is to ensure that the change on the [[Archetype Instance]]s can be applied properly and survive [[Blueprint Reconstruction]].

## Editing Primitive Component Properties

The following example demonstrates how to change a property named `MyProperty` of type `double`, or any primitive type, in a [[Component]] of type `UMyComponent`.
```cpp
void MakeChange(UMyComponent& Component, double NewValue)
{
	// Start the undo/redo transaction. Any change we register from now until the end
	// of the scope will be included in the undo/redo operation.
	FScopeTransaction Transaction(LOCTEXT("MakeChangeUndo", "Make Change"));

	// Find the property that we are about to modify. The property object is our
	// handle into the Unreal Engine reflection system and how we commuicate to
	// Unreal what we are about to change. In this example the propery is a primitive
	// type with no nesting so the property chain has a single element.
	FProperty* Property =
		UMyComponent::StaticClass()->FindPropertyByName(
			GET_MEMBER_NAME_CHECKED(UMyComponent, MyProperty));
	FEditPropertyChain EditPropertyChain;
	EditPropertyChain.AddHead(Property);
	EditPropertyChain.SetActivePropertyNode(Property);

	// Notify the object that its property is about to be modified. This will prepare
	// the object and any Archetype Instances for inclusing in the undo / redo transaction.
	//
	// This will also call Modify on the object and any Archetype Instances, which will
	// store the current state to the transaction buffer.
	((UObject&)Component)->PreEditChange(EditPropertyChain);

	// Make the actual change.
	Component.SetMyProperty(NewValue);

	// Notify the object that the change is complete. This will call PropagatePostEditChange
	// on the Archetype Instances.
	FPropertyChangedChainEvent ChainEvent(PropertyChain, Event);
	Component.PostEditChangeChainProperty(ChainEvent);
}
```


## Editing Array Component Properties

