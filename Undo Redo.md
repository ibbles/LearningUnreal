Undo-redo regions are created from C++ by creating an `FScopedTransaction` on the stack.
The name of the transaction is displayed in the undo history and in a popup when being undone.
All marked modifications done while the `FScopedTransaction` is active become a single undo step.
An operation is considered marked if it modifies a `UPROPERTY` and `Modify` has been called on the
modified object before the modification is done.
There may also be something about whether or not the `UPROPERTY` is replicated. Not sure.
`Modify` is a member function of `UObject`.
Example:
```c++
UCLASS()
class UActorMover : public UObject
{
public:
    GENERATED_BODY();

    UPROPERTY()
    AActory* MyActor {nullptr};

    UPROPERTY()
    int32 NumMoves {0};

    void MoveActor();
}

void UMyClass::MoveActor()
{
    // Start a new undo transaction. Any marked changes made until
    // this object goes out of scope is considered a single atomic
    // operation in the undo history.
    FScopedTransaction Transaction(
        LOCTEXT("MoveActor", "Move actor."));

    // This member function will make two changes, one to itself and
    // one on the Actor it has a pointer to.

    // Mark this object as about to be modified.
    Modify();

    // Make the modification. NumMoves is a `UPROPERTY` on an objects on
    // which Modify has been called so it will be included in the undo
    // history.
    ++NumMoves;

    // Next we do the move. First mark the Actor for modification and then
    // do the actual modification. AActor is responsible for doing any
    // propagation of the Modify to sub-objects that may be necessary in
    // case the modifying member function, SetActorLocation in this case,
    // modifies a sub-object.
    MyActor->Modify();
    MyActor->SetActorLocation();

}
```

I believe undo only applies to objects with the `RF_Transactional` flag set.
A pattern I've seen is that the undo management is handled by GUI-level code such as Component Visualizers and Details Customizations, not by the modified object itself.

```c++
FScopedTransaction transaction(); // will open up a slot in for data in GEditor
Modify(); // On what should this be called? The FScopedTransaction? Or is `Modify` a stand-in name for any modification on any object?
// Do you modifications.

//~FScopedTransaction() will finish the transaction, called when `transaction` goes out of scope.
```

# Undo Redo When Editing Blueprints With C++

Extra care must be taken to support undo / redo when editing [[Blueprint]]s from C++.
This is because Blueprint sub-objects, such as the [[Component]]s we have added to an Actor Blueprint, can have [[Archetype Instance]] that should also be updated both during the initial edit and during the undo / redo operations.

