[[Large World Coordinates]] is the name of a collection of implementation details and systems in Unreal Engine to support very large worlds.
[[Large World Coordinates]] was introduced with Unreal Engine 5.0 and got a major update with Unreal Engine 5.4.
Some of this text may need to change for 5.4 and later engine versions.

[[Niagara]] does support large world coordinates, but in a limited capacity.
Coordinates on the CPU are stored as 64-bit floating-point values while coordinates on the GPU are 32-bit relative to some offset in the world.
This means that particles can be far away from the world origin, but not far away from each other.

For example, this is the code used by Unreal Engine when transferring a position from the CPU to the GPU:
```cpp
FVector3f FNiagaraLWCConverter::ConvertWorldToSimulationVector(FVector WorldPosition) const
{
	return FVector3f(WorldPosition - SystemWorldPos);
}

FNiagaraPosition FNiagaraLWCConverter::ConvertWorldToSimulationPosition(FVector WorldPosition) const
{
	return FNiagaraPosition(ConvertWorldToSimulationVector(WorldPosition));
}
```
From [`NiagaraType.cpp`](https://github.com/EpicGames/UnrealEngine/blob/5.3.2-release/Engine/Plugins/FX/Niagara/Source/Niagara/Private/NiagaraTypes.cpp#L151) for Unreal Engine 5.3.2.

Notice that an `FVector3f`, a 32-bit type, is returned and there is a `SystemWorldPos` that the returned position is expressed relative to.
`SystemWorldPos` is the world position of the lower corner of the tile that the Niagara system was in when it was activated.
I think the tile, and thus the `SystemWorldPos`, is updated when the [[Niagara Component]] is moved between tiles.

With [[Large World Coordinates]] the world is divided into tiles.
A tile is a 3D cube and the world is filled uniformly and regularly with such cubes.
A tile is by default [2097152](https://github.com/EpicGames/UnrealEngine/blob/9556237be08ab2e8a5eb7ae01107ece0596c5e3c/Engine/Source/Runtime/Core/Public/Misc/LargeWorldRenderPosition.h) units, i.e. cm, large, which is about 21 km.
This gives a worst-case 32-bit floating-precision of 0.125 units at the edge of each tile.

Support for [[Large World Coordinates]] in [[Niagara]] can be disabled for the entire project, or per [[Niagara System]].
- [[Project Settings]] > Plugins > Niagara > System Support Large World Coordinates.
	- If disabled here then it is disabled for ALL [[Niagara System]]s, it can't be forced-on per-system. (I think.)
- [[Niagara Editor]] > System Properties > Rendering > Support Large World Coordinates.

# References

- [_Large World Coordinates in Niagara_ by Epic Games at dev.epicgames.com/documentation UE5.3](https://dev.epicgames.com/documentation/en-us/unreal-engine/large-world-coordinates-in-niagara-for-unreal-engine?application_version=5.3)
- [_FNiagaraLWCConverter::ConvertWorldToSimulationPosition_ by Epic Games at github.com UE5.3](https://github.com/EpicGames/UnrealEngine/blob/5.3.2-release/Engine/Plugins/FX/Niagara/Source/Niagara/Private/NiagaraTypes.cpp#L151)
- [Reply to _Follow up question about precision issues at larger world coordinates_ by Wouter De Keersmaecker (Epic Games) at udn.unrealengine.com UE5.0](https://udn.unrealengine.com/s/question/0D54z00009c8StJCAU/follow-up-question-about-precision-issues-at-larger-world-coordinates)
- [Replies to _Niagara position precision_ by Michael Galetzka (Epic Games) at udn.unrealengine.com UE5.1](https://udn.unrealengine.com/s/question/0D54z00009aHYO9CAO/niagara-position-precision)

