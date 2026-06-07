This note acts as an entry point into learning how to write C++ PCG nodes.
It uses the built-in Create Points node as the basis for the explanation since that is one of the simplest possible nodes.
It has not inputs and produces a set of output points based solely on properties set on the node itself.
By studying this node we can learn about to a PCG node is declared, what types and functions we need to implement, how to represent point data, and how to declare and write output.

For a wider introduction to the Procedural Content Generation framework, see [[Procedural Content Generation PCG]].

The implementation for the Create Points node is in `PCGCreatePoints.h` and `PCGCreatePoints.cpp`.
I would like to explore and annotate the file in full here, but the Unreal Engine license doesn't allow such large segments of source code to be reproduced so you will have to follow along in the file separately, either from your Unreal Engine installation or on GitHub, [`PCGCreatePoints.h`](https://github.com/EpicGames/UnrealEngine/blob/5.7.4-release/Engine/Plugins/PCG/Source/PCG/Public/Elements/PCGCreatePoints.h) and [`PCGCreatePoints.cpp`](https://github.com/EpicGames/UnrealEngine/blob/5.7.4-release/Engine/Plugins/PCG/Source/PCG/Private/Elements/PCGCreatePoints.cpp).

# Main Classes

The two main base classes when creating a new PCG node are `UPCGSettings` and `IPCGElement`.
`UPCGSettings` controls the what the node _is_ and `IPCGElement` controls what the node _does_.
A common naming convention is to declare types named according to the pattern `UPCGMyNodeSettings` and `FPCGMyModeElement`.
For the Create Points node that becomes `UPCGCreatePointsSettings` and `FPCGCreatePointsElement`.

## `UPCGSettings`

One of the responsibilities of `UPCGSettings` is to tell Unreal Editor how the node should be displayed in the UI.


## `IPCGElement`
