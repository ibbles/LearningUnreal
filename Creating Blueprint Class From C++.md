```cpp
//Creating a blueprint

UBlueprintFactory* Factory = NewObject<UBlueprintFactory>();
IAssetTools& AssetTools = FModuleManager::GetModuleChecked<FAssetToolsModule>("AssetTools").Get();

UObject* Object = nullptr;
Object = AssetTools.CreateAsset(
NewFileName,
NewFilePath,
UBlueprint::StaticClass(),
Cast<UFactory>(Factory)
);

UBlueprint* Blueprint = Cast<UBlueprint>(Object);

// Adding a Hierarichal Instance Static Mesh, but I believe this can be any actorcomponents (Not tried)
USCS_Node* Node = Blueprint->SimpleConstructionScript->CreateNode(UHierarchicalInstancedStaticMeshComponent::StaticClass(), TEXT("HISM"));
Blueprint->SimpleConstructionScript->AddNode(Node);
FKismetEditorUtilities::CompileBlueprint(Blueprint);
```
[Adding Components and modifying properties of UBlueprint asset @ forums.unrealengine.com](https://forums.unrealengine.com/t/adding-components-and-modifying-properties-of-ublueprint-asset/41992/7)

TODO: Read https://udn.unrealengine.com/s/question/0D5QP000005QQPb0AO/what-is-the-correct-way-to-manipulate-blueprint-assets-from-c
