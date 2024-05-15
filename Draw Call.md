A draw call is an instruction sent from the CPU to the GPU to render something.
That something can be a [[Static Mesh]], a [[Skeletal Mesh]], or something else.

The number of draw calls can have a significant impact on performance.
More draw calls generally means higher frame times and lower frame rate.
See also [[Static Mesh Performance]] and [[Rendering Performance Profiling And Analysis]].

A [[Static Mesh]] need one draw call for every [[Material]] it has.
Try to keep the number of material slots needed for a [[Static Mesh]] as low as possible.
Each [[Static Mesh Component]] added to the scene causes a new set of draw calls, one for each [[Material]] on that mesh.
Unless [[Nanite]] is used, which makes things more complicated.
Performance can be improved by having multiple [[Material|Materials]] only on the highest detail LODs,
and having fewer [[Material|Materials]] on the more coarse-grained LOD levels.
We can batch render [[Static Mesh]] rendering with [[Instanced Static Mesh]],
which can render multiple mesh instances with a single set of draw calls.


# References

- [_Becoming an Environment Artist in Unreal Engine_ > _Polycount, Optimization and Draw Calls_ by Epic Online Learning @ dev.epicgames.com/courses 2020 UE4.25](https://dev.epicgames.com/community/learning/courses/Gm/becoming-an-environment-artist-in-unreal-engine/r2n/unreal-engine-polycount-optimization-and-draw-calls)

