A Grid2D Collection is an array of 2D buffers, kinda like a [[Render Target]] but more flexible.
Each buffer contains "attributes".
The buffers can be iterated over.
This lets us implement grid based algorithms.
Often used together with [[Niagara Simulation Stage]].

With a Grid2D the [[Niagara Module]] can decide which index to write to, we are not locked to a 1:1 relationship between threads and pixels as we are with [[Material]] and [[Render Target]].

There is something about a Sim Grid ([_The Art of Illusion - Niagara Simulation Framework Overview_ 8:14](https://youtu.be/-cKgZrrBJ2w?t=494)) on which Preview Attribute can be enabled.
If this and Enable GPU Compute Debug on the [[Niagara System]] is enabled then the Grid2D for the selected Attribute will be rendered in the [[Level Viewport]].


# References

- [_UE4 Niagara Grid2d Quickstart_ by Adreas Glad @ youtube.com, 2021](https://www.youtube.com/watch?v=XVKpofOj44c)
- [_The Art of Illusion - Niagara Simulation Framework Overview_ by Asher Zhu @ youtube.com 2020](https://youtu.be/-cKgZrrBJ2w)
	- [_Grid2D Collection_](https://youtu.be/-cKgZrrBJ2w?t=181)