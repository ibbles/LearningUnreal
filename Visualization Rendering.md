The usual way to render things is to create a rendering Component, such as a [[Static Mesh Component]] or a [[Skeletal Mesh Component]], and configure that appropriately.
Sometimes we wish to rendering things that aren't meant to be seen by the player, debug- or visualization type rendering.
An example is the navigation mesh visualization built into Unreal Editor.
The purpose of this note is to explore how such rendering can be performed, using the navigation mesh visualization as a starting point.
My hope is that non-engine code can do rendering in a similar way.
Other rendering code may be explored in later sections of this note.
The code analysis is done on Unreal Engine 4.26.


# Navigation

## Summary

There is an [[Actor]], `ARecastNavmesh`, that has an `UPrimitiveComponent* RenderingComp`.
`RenderingComp` is initialized with `NewObject<UNavMeshRenderingComponent>` in
- `PostInitProperties`
- `PostLoad`
- `RerunConstructionScripts`
- `OnRegistered`
- `PostEditUndo`

There is a Component, `UNavMeshRenderingComponent`, that handles rendering.
It inherits, eventually, from `UPrimitiveComponent`, which makes it visible to the Unreal Engine rendering system.
It implements `CreateSceneProxy` that creates an `FNavMeshSceneProxy`.
It implements `OnRegister`, where it set a timer to periodically check the `show` flag.
It implements `OnUnregister`, where it removes the timer.
It implements `(Create|Destroy)RenderState_Concurrent`, where it registers a debug draw delegate. Don't know what this is for.
It implements `CalcBounds`, where it creates a bound if `bDrawOctree` is set. Not sure why the bound can be empty for everything else.
It has a helper function `GatherData` that `FNavMeshSceneProxy` calls to get lines and vertices and such.

Unreal Engine uses Scene Proxies to store data used by the rendering thread.
The nav mesh Rendering Component creates a `FNavMeshSceneProxy` in `CreateSceneProxy`.
The Scene Proxy implements `GetDynamicMeshElements` to generate lines and triangles.


## Creation

The navigation mesh can be shown in-editor by selecting Level Viewport > Show > Navigation.
It shows up as green surfaces.

A navigation volume, in which a navigation mesh is created, is defined by creating a Nav Mesh Bounds Volume Actor.
This also creates a Recast Nav Mesh Actor.
A single Recast Nav Mesh Actor pulls from all Nav Mesh Bounds Volume Actors in the level.
Recast Nav Mesh Actor contains settings for what should be rendered when Show > Navigation is enabled in [[Details Panel]] > Display.


## Enable rendering

`EditorShowFlags.cpp` does `EngineFlagsChords.Add("Navigation", FInputChords(EKeys::P)`.
This matches with the keyboard shortcut listed in the Show listing.
`EngineFlagsChords` deal with something called `FEngineShowFlags`.
That worries me a bit, it sounds very in-engine-y, and not pluggable.

## Rendering Component `UNavMeshRenderingComponent`

There is a Component named `UNavMeshRenderingComponent` that inherits from `UPrimitiveComponent`.
It has a companion class named `FNavMeshDebugDrawDelegateHelper` inheriting from `FDebugDrawDelegateHelper`.
Not sure what that is. Looks text related.
It has a companion class named `FNavMeshSceneProxy` inheriting from `FDebugRenderSceneProxy`.
It has a companion class named `FNavMeshSceneProxyData`.


### `FNavMeshSceneProxyData

Contains a struct named `FDebugMeshData` holding
- `TArray<FDynamicMeshVertex> Vertices`
- `TArray<uint32> Indices`
- `FColor ClusterColor`

Contains a struct named `FDebugPoint` holding
- `FVector Position`
- `FColor Color`
- `float Size`

Contains a bunch of `TArray<FDebugRenderSceneProxy::FDebugLine>`.
Contains a `TArray<FDebugRenderSceneProxy::FMesh>`.

It does not contain a lot of functions.
It does have `GatherData`.
`GatherData` is big.
Contains a bunch of `bool bGather.+ = HasFlag(.+); if (bGather.+) {}`-blocks.
`ENavMeshDetailFlags::FilledPolys` and `ENavMeshDetailFlags::BoundaryEdges` are interesting.


#### `ENavMeshDetailFlags::BoundaryEdges`

`FNavMeshSceneProxyData::GatherData`:
Has an `FRecastDebugGeometry NavMeshGeometry;`. 
Sets `NavMeshGeometry.bGatherNavMeshEdges`.
`NavMeshGeometry.NavMeshEdges` is filled in by `ARecastNavMesh::GetDebugGeometry`.
Generates an `FColor`.
Keeps colors in `NavMeshColors`.
Has a `TArray<FVector>& NavMeshEdgeVerts` initialized from `NavMeshGeometry.NavMeshEdges`.
This array holds the vertices for the perimeters of the nav mesh.
Loops over `NavMeshEdgeVerts` from the `FRecastDebugGeometry` filled in by  `ARecastNavMesh::GetDebugGeometry`.
Adds a line to `NavMeshEdgeLines` for very pair of vertices in `NavMeshEdgeVerts`.
`NavMeshEdgeLines` is a `TArray<FDEbugRenderSceneProxy::FDebugLine>` in `FNavMeshProxyData`.
That marks the end of `FNavMeshSceneProxyData::GatherData`.

`FNavMeshSceneProxy::GetDynamicMeshElements`:
This is a virtual function on `FPrimitiveSceneProxy`.
It is called from the rendering thread, meaning that it may not touch UObjects, only their proxies, such as `FNavMeshSceneProxy`.
Calls the base class implementation, `FDebugRenderSceneProxy::GetDynamicMeshElements`.
Does a bunch of visibility checks.
Uses `FPrimitiveDrawInterface* PDI = Collector.GetPDI(ViewIndex);`.
`Collector` is an `FMeshElementCollector`.
Reserves a bunch of lines with `PDI->AddReserveLines(..., Num, ...);`.
This function doesn't have any documentation.
Loops over the lines in `FNavMeshSceneProxyData ProxyData`.
Cull if not in view.
`PDI->DrawLine` either as `SDPG_World` or `SDPG_Foreground` depending on `FNavMeshRenderingHelpers::LineInCorrectDistance`.


#### `ENavMeshDetailFlags::FilledPolys`

##### `FNavMeshSceneProxyData::GatherData`

This part of `FNavMeshSceneProxyData::GatherData` fills in the internals of the nav mesh.
It creates shaded triangles.
It creates an `FDebugMeshData DebugMeshData` to hold the data.
This is a structure that is defined inside `FNavMeshSceneProxyData`.
```cpp
struct FDebugMeshData
{
    TArray<FDynamicMeshVertex> Vertices;
    TArray<uint32> Indices;
    FColor ClusterColor;
};
TArray<FDebugMeshData> MeshBuilders;
```

It loops over `MeshVerts`.
Declared as `const TArray<FVector>& MeshVerts = NavMeshGeometry.MeshVerts;`.
`NavMeshGeometry` is filled in by `ARecastNavMesh::GetDebugGeometry` earlier in `GatherData`.
Using the data in `MeshVerts` triangles are built using `FNavMeshRenderingHelpers::AddVertex`.
I assume this adds stuff to `FDebugMeshData::Vertices`.

It also loops over `MeshIndices`.
Declared as `const TArray<int32>& MeshIndices = NavMeshGeometry.Clusters[Idx].MeshIndices;`.
`NavMeshGeometry` is filled in by `ARecastNavMesh::GetDebugGeometry` earlier in `GatherData`.
Using the data in `MeshIndices` triangles are built using `FNavMeshRenderingHelpers::AddTriangleIndices`.
I assume his adds stuff to `FDebugMeshData::Indices`.

`FDebugMeshData::ClusterColor` is assigned a color. I don't care how it selects the color.

Finally the `DebugMeshData` is added with `MeshBuilders.Add(DebugMeshData);`.

This marks the end of `FNavMeshSceneProxyData::GatherData`.
`GatherData` was called from `UNavMeshRenderingComponent::CreateSceneProxy`.


##### `FNavMeshSceneProxy::FNavMeshSceneProxy`

The next time we see the `MeshBuilders` being used is in `FNavMeshSceneProxy::FNavMeshSceneProxy`.
They are used to populate
- `TArray<FDynamicMeshVertex> Vertices`, a local variable.
- `TArray<FMeshBatchElement> MeshBatchElements`, a member variable of `FNavMeshSceneProxy`.
- `TArray<FColoredMaterialRenderProxy> MeshColors`, a member variable of `FNavMeshSceneProxy`.
- `FDynamicMeshIndexBuffer32 IndexBuffer`, a member variable of `FNavMeshSceneProxy`.
- `FStaticMeshVertexBuffers VertexBuffers`, a member variable of `FNavMeshSceneProxy`.

For every mesh a `FMeshBatchElement Element` is created on the stack.
On it `FirstIndex`, `NumPrimitives`, `MinVertexIndex`, `MaxVertexIndex`, and `IndexBuffer` are set.
Then the `Element` is added to the `MeshBatchElements` member variable.

An `FColoredMaterialRenderProxy` is added to the `MeshColors` member variable.
This involves
- an `FMaterialRenderProxy` created from `GEngine->DebugMeshMaterial->GetRenderProxy()`.
- the `ClusterColor` stored in `FDebugMeshData`.

The `Vertices` from the `FDebugMeshData` is appended to the local `Vertices` array.
The `Indices` from the `FDebugMeshData` is added to the member variable `IndexBuffer`,
but each index is offsetted to account for the vertices that have already been added to `Vertices`.

After all `FDebugMeshData` objects have been added/appended to `MeshBatchElements`, `MeshColors`, `Vertices`, and `Indices`,
an extra `FColoredMaterialRenderProxy` is added to `MeshColors`. Don't know why.

The member `VertexBuffers` is initialized using `InitFromDynamicVertex`, passing in both a `VertexFactory` and the local `Vertices`.
The member `IndexBuffer` is initialized with `BeginInitResource`.

That marks the end of `FNavMeshSceneProxy::FNavMeshSceneProxy`.


##### `FNavMeshSceneProxy::GetDynamicMeshElements`

The next step in the mesh rendering is to hand over the `MeshBatchElements` to the `FMeshElementCollector` passed to `GetDynamicMeshElements`.
`GetDynamicMeshElements` loops over the given Views and checks visibility.
For every view `GetDynamicMeshElements` loops over `FNavMeshSceneProxy::MeshBatchElements`.
An `FMeshBatch Mesh` is allocated with `Collector.AllocateMesh()`.
An `FMeshBatch` contains at least one `FMeshBatchElement` in the `Elements` member array.
We get it with `Mesh.Elements[0]`.
To that `FMeshBatchElement` we assign the current element in `FNavMeshSceneProxy::MeshBatchElements`.

We have now allocated an `FMeshBatch` from the Collector and initialized its first `FMeshBatchElement` in `FMeshBatch::Elements`.

Next we allocate a `FDynamicPrimitiveUniformBuffer` from the Collector and give to the `BatchElement`.

A bunch of parameters are set on the `FMeshBatch`.
In particular, we give it a pointer to `FLocalVertexFactory FNavMeshSceneProxy::VertexFactory`.

Lastly the `FMeshBatch` is given to the Collector with `Collector.AddMesh`.



### `FDebugRenderSceneProxy : public FPrimitiveSceneProxy`

`FPrimitiveSceneProxy` is the base class for most scene proxies.
A scene proxy is the rendering thread side of something that can be rendered, such as a [[Static Mesh]].
`FDebugRenderSceneProxy` contains a bunch of shape primitives to render.
Such as lines, spheres, boxes, cylinders, and meshes.
These are read in `FDebugRenderSceneProxy::GetDynamicMeshElements`.
Often with `DrawWire.+(PDI, ...)` or `Get.+Mesh(, ..., Collector)`.
The pattern seems to be that `PDI` is used to draw lines and `Collector` to draw meshes.
The `Get.+Mesh` functions, in `PrimitiveDrawingUtils.cpp`, use `FDynamicMeshBuilder` create a mesh and give to the Collector.
```cpp
TArray<FDynamicMeshVertex> MeshVerts;
TArray<uint32> MeshIndices;
/* Fill in MeshVerts and MeshIndices to produce whatever mesh you need. */
MeshBuilder.GetMesh(ModelToWorldMatrix, MaterialRenderProxy, ..., Collector);
```

I don't yet know what a `MaterialRenderProxy` is, or how to get or create one.


### `FNavMeshSceneProxy : public FDebugRenderSceneProxy

Has a `FNavMeshSceneProxyData`.
Has a  `FDynamicMeshIndexBuffer32`.
Has a `FStaticMeshVertexBuffers`.
Has a `FLocalVertexFactory`.
Has a `TArray<FMeshBatchElement>`.


### `FNavMeshDebugDrawDelegateHelper : public FDebugDrawDelegateHelper`

Seems text-related.


### `UNavMeshRenderingComponent : public UPrimitiveComponent`

Overrides `CreateSceneProxy`, `On(Un)?Register`, `(Create|Destroy)RenderState_Concurrent`, `CalcBounds`, and `GatherData`.


## RecastNavMesh Actor


### `ANavigationData : public AActor`

Has a `UPrimitiveComponent* RenderingComp;`.
Has `virtual UPrimitiveComponent* ConstructRenderingComponent() { return NULL; }`.
Has `void InstantiateAndRegisterRenderingComponent();`.

`ConstructRenderingComponent` is called from `PostInitProperties`, if `!HasAnyFlags(RF_ClassDefaultOBject)`, and `InstantiateAndRegisterRenderingComponent`.

`InstantiateAndRegisterRenderingComponent` is called from `PostLoad`, `RerunConstructionScripts`, `OnRegistered`.

`InstantiateAndRegisterRenderingComponent` calls `ConstructRenderingComponent`, calls `RegisterComponent` on it, and maybe sets it as the root Component.


### `FRecastDebugGeometry`

Rendering data filled in by `FNavMeshSceneProxyData::GatherData`.
(
No, maybe not. Looks like `FNavMeshSceneProxyData::GatherData` reads from this.
)
Has a `TArray<FVector> NavMeshEdges;`.


### `ARecastNavMesh : public ANavigationData`

Contains a bunch of `uint32 bDraw.+:1;`, such as `bDrawTriangleEdges`.
These correspond to the checkboxes in the Display category of the Actor's [[Details Panel]].
Uses a `FRecastDebugGeometry`.

Does not contain a `UNavMeshRenderingComponent`.
Overrides `ConstructRenderingComponent` from `ANavigationData`.
Creates a `UNavMeshRenderingComponent` in `ARecastNavMesh::ConstructRenderingComponent`.
Casts the inherited `ANavigationData::RenderComp` to a `UNavMeshRenderingComponent`.

Drawing updates are requested from multiple places.
Uses `FSimpleDelegateGraphTask::CreateAndDispatchWhenReady` to request drawing update.
To ensure that it is run on the game thread, I think.
Ultimately calls `RenderingComp->MarkRenderStateDirty()`.


# Minimal example, bottom up

This minimal example demonstrates how to render a few triangles using a Rendering Component with a Scene Proxy.
The triangles are assumed to be dynamic, i.e. updated every frame.

## Scene Proxy

`MySceneProxy.h`:
```cpp
#include "PrimitiveSceneProxy.h"

class FMySceneProxy : public FPrimitiveSceneProxy
{
public:
	using Super = FPrimitiveSceneProxy();

	virtual void GetDynamicMeshElements(
		const TArray<const FSceneView*>& Views, const FSceneViewFamily& ViewFamily,
		uint32 VisibilityMap, FMeshElementCollector& Collector) const override;
}
```

`MySceneProxy.cpp`:
```cpp
#include "MySceneProxy.h"

#include "DynamicMeshBuilder.h"

int32 AddVertex(TArray<FDynamicMeshVertex>& Vertices, const FVector& Location, const FColor& Color)
{
	const int32 Index = Vertices.Num();
	FDynamicMeshMeshVertex& Vertex = Vertices.AddDefaulted_GetRef();
	Vertex.Position = Location;
	Vertex.TextureCoordinate[0] = FVector2D::ZeroVector;
	// Not sure what the rest does.
	Vertex.TangentX = FVector(1.0f, 0.0f, 0.0f);
	Vertex.TangentZ = FVector(0.0f, 1.0f, 0.0f);
	Vertex.TangentZ.Vector.W = 127;
	Vertex.Color = Color;
	return Index;
}

void AddTriangleIndices(TArray<uint32>& Indices, int32 V0, int32 V1, int32 V2)
{
	Indices.Add(V0);
	Indices.Add(V1);
	Indices.Add(V2);
}

void FMySceneProxy::GetDynamicMeshElements(
	const TArray<const FSceneView*>& Views, const FSceneViewFamily& ViewFamily,
	uint32 VisibilityMap, FMeshElementCollector& Collector)
{
	Super::GetDynamicMeshElements(Views, ViewFamily, VisibilityMap, Collector);

	// FDebugMeshData.
	TArray<FDynamicMeshVertex> Vertices;
	TArray<uint32> Indices;
	FColor Color;

	const int32 Near = AddVertex(Vertices, FVector(0.0f, 0.0f, 0.0f), FColor::Green);
	const int32 Far = AddVertex(Vertices, FVector(1.0f, 0.0f, 0.0f), FColor::Green);
	const int32 Right = AddVertex(Vertices, FVector(0.5f, 1.0f, 0.0f), FColor::Green);
	const int32 Top = AddVertex(Vertices, FVector(0.5f, 0.5f, 1.0f), FColor::Blue);

	AddTriangleIndices(Near, Top, Far);
	AddTriangleIndices(Near, Right, Top);
	AddTriangleIndices(Near, Far, Right);
	AddTriangleIndices(Far, Top, Right);

	FMeshBatchElement Element;
	Element.FirstIndex = 0;  // Index in the index buffer where this batch starts.
	Element.NumPrimitives = 4;  // We have four triangles.
	Element.MinVertexIndex = 0;  // Index in the vertex buffer where this batch starts.
	Element.MaxVertexIndex = 3;  // Index in the vertex buffer where this batch ends. Not one-past.
	Element.IndexBuffer = &s

	for (int32 ViewIndex = 0; ViewIndex < Views.Num(); ++ViewIndex)
	{
		if (!(VisibilityMap & (1 << ViewIndex)))
		{
			continue;
		}

		FMeshBatch& Mesh = Collector.AllocateMesh();
		FMeshBatchElement& BatchElement = Mesh.Elements[0];
		BatchElement = MeshBatchElement;
	}
}
```


## Rendering Component

`MyRenderingComponent.h`:
```cpp
#include "Components/PrimitiveComponent.h"

UCLASS()
class UMyRenderingComponent : public UPrimitiveComponent
{
	GENERATED_BODY();

public:
	//~ Begin UPrimitiveComponent Interface
	virtual FPrimitiveSceneProxy* CreateSceneProxy() override;
	//~ End UPrimitiveComponent Interface

	//~ Begin USceneComponent Interface
	virtual FBoxSphereBounds CalcBounds(const FTransform &LocalToWorld) const override;
	//~ End USceneComponent Interface
};
```

`MyRenderingComponent.cpp`:
```cpp
#include "MyRenderingComponent.h"

FPRimitiveSceneProxy* UMyRenderingComponent::CreateSceneProxy()
{

}

FBoxSphereBounds UMyRenderingComponent::CalcBounds(const FTransform& LocalToWorld)
{

}
```


## Actor

`MyActor.h`:
```cpp
UCLASS()
class AMyActor : public AActor
{
	GENERATED_BODY();
public:
	//~ Begin UObject Interface.
	virtual void PostInitProperties() override;
	virtual void PostLoad() override;
#if WITH_EDITOR
	virtual void PostEditUndo() override;
#endif
	//~ End UObject Interface.

	//~ Begin AActor Interface.
	void RerunConstructionScripts();
	//~ End AActor Interface.

private:
	void ConstructRenderingComponent();
	void ConstructAndRegisterRenderingComponent();

private:
	UMyRenderingComponent* RenderingComponent;
};
```


`MyActor.cpp`:
```cpp
#include "MyActor.h"

void AMyActor::PostInitProperties()
{
	Super::PostInitProperties();
	if (IsPendingKill())
	{
		return;
	}

	if (!HasAnyFlags(RF_ClassDefaultObject))
	{
		ConstructRenderingComponent();
	}
}

void AMyActor::PostLoad()
{
	Super::PostLoad();
	ConstructAndRegisterRenderingComponent();
}

void AMyActor::PostEditUndo()
{
	ConstructAndRegisterRenderingComponent();
	Super::PostEditUndo();
}

void AMyActor::RerunConstructionScripts()
{
	Super::RerunConstructionScripts();
	ConstructAndRegisterRenderingComponent();
}

void AMyActor::ConstructRenderingComponent()
{
	RenderingComponent = NewObject<URenderingComponent>(this, TEXT("Rendering Component"), RF_Transient);
}

void AMyActor::ConstructAndRegisterRenderingComponent()
{
	if (IsPendingKill())
	{
		// We are about to be destroyed, no need to create new stuff.
		return;
	}

	if (RenderingComponent != nullptr && !RenderingComponent->IsPendingKill())
	{
		// We already have a valid Rendering Component, no need to replace it.
		return;
	}

	if (RenderingComponent != nullptr)
	{
		// Rename the old pending kill Rendering Component so we can reuse the name.
		// Not sure which of the flags are really necessary, or even what they do.
		RenderingComponent->Rename(
		nullptr, nullptr,
		REN_DontCreateRedirectors | REN_ForceGlobalUnique | REN_DoNotDirty
			| REN_NonTransactional | REN_ForceNoResetLoaders);
	}

	ConstructRenderingComponent();

	UWorld* World = GetWorld();
	if (World != nullptr && World->bIsWorldInitialized)
	{
		RenderingComponent->RegisterComponent();
	}
}
```
