A Landscape is one way to create the ground in our environment.
It is a height field, which means that we can do hills and valleys but not caves or overhangs.
A Landscape is either sculpted using paintbrush-like tools or with the [[Landmass]] tool.

A Landscape is a hierarchical structure.
The top level is the Landscape itself.
The Landscape consists of Landscape Components.
Each Landscape Component consists of a number of Sections.
Each Section consists of a number of Quads.

Most of the operations we can perform on a Landscape are accessed from the [[Landscape Mode]].

See [[Creating a Landscape]].

The height map can be exported to a PNG using [[Landscape Mode]] > Sculpt tab > Sculpt tool > Landscape panel > Target Layers > right-click a layer > Export To File.
The resulting PNG can be used when [[Creating a Landscape]].

From a Landscape's [[Details Panel]] we can set a [[Landscape Material]].
There is no special [[Landscape Material]] type or Used With Landscapes flag, any [[Material]] will work.
Though there are some Landscape specific nodes the [[Material]] can use, such as [[Landscape Blend Layers Material|Landscape Layer Blend]].
Layers are assigned to specific patches of the Landscape with [[Paint Material on a Landscape|Landscape Painting]].

See also:
- [[Quixel Megascans Materials on Landscape]]
- [[Create a Photorealistic world in UE4]]
- [[Getting Started with Landscapes]]

# References

- [UE4: Step-by-Step to Your First Landscape for Complete Beginners (Day 1/3: 3-Day Tutorial Series @ youtube.com by WorldofLevelDesign)](https://www.youtube.com/watch?v=CWHV8-cVYTA)
- [_Advanced Skill Sets for Environment Art_ > _Landscape Sculpting and Landmass_ by Epic Online Learning 2022 UE4.27](https://dev.epicgames.com/community/learning/courses/Qwa/unreal-engine-advanced-skill-sets-for-environment-art/0Rz6/landscape-sculpting-and-landmass)

