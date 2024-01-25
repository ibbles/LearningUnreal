A [[Material]] has a bunch of inputs.
Some, such as texture coordinates and vertex colors, come from the mesh being rendered.
Some, such as camera position and time, come from the engine.
Some, the [[Material Parameter|Material Parameters]], are user provided.
The author of the Material creates the parameters and implement their semantics through the [[Material Graph]].
Expression nodes in the [[Material Graph]] that represents an input is often red or brown.
Red for engine provided data.
Brown for user provided data.

Example engine inputs:
- Absolute World Position
	- The world position of the pixel currently being shaded.
- Actor Position
	- The world location of the Actor that owns the object currently being rendered.
- Camera Vector.
	- Not sure. Explained as "It looks at the rotation of the camera, of the viewport, or the game or whatever.". I don't understand.
	- Can be used with a cube map.
- Pixel Depth
	- The distance from the camera to the pixel currently being shaded, in cm.
- Vertex Color
	- The color assigned to the current vertex, or a blend of the three colors of the three vertices of the current triangle.
- Vertex Normal WS
	- The normal stored with the currently rendered vertex transformed to world space.
	- Not sure what this does in a pixel shader, how it would be different from Pixel Normal WS.
- Object Bounds
	- The bounding box of the object the  [[Material]] is applied to.
	- Not sure if this is in local space or world space.
- Object Radius
	- The radius of the local bounding sphere of the object the [[Material]] is applied to.
- Pixel Normal WS
	- The normal of the currently shaded pixel in world space.
- Reflection Vector
	- The camera vector reflected around the normal.
	- (I think, not sure on this one. Explained as "Like camera vector but inverted in the upwards direction". I don't understand that.)
	- When used with a [[Texture Sample]] from a [[Cube Map]] we get the reflection effect.


# References

- [_Materials Master Learning_ > _Combinations Using External Information_ by Epic Games, Sjoerd de Jong @ dev.epicgames.com 2019](https://dev.epicgames.com/community/learning/courses/2dy/unreal-engine-materials-master-learning/5zp/combinations-using-external-information)
