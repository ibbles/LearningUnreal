The Niagara Editor is where we author [[Niagara]] particle effects.
The Niagara Editor is used to edit:
- [[Niagara System]]
- [[Niagara Editor]]
- Anything else? Should [[Niagara Module]] be listed here?
It consists of
- a Preview panel with a [[Viewport]]
- a Parameters panel with parameters.
- a User Parameters panel with more parameter.
- a System Overview panel with nodes representing the [[Niagara System]] itself and the [[Niagara Emitter]] that it contains.
- a Selection panel that is similar to a [[Details Panel]] in that it shows the settings and properties for the currently selected object.

# Timeline

Scrub back and forth in time to see the particle effect evolving.
When moving forwards in time the particle system is simulated forward as usual.
When moving backwards in time the particle system is simulated from t=0.0 every time the timeline head is moved.
If the particle system is not deterministic then there can be large differences in particle configuration even for very small steps in time since two completely different simulations was used to arrive at these two points in time.
There is a Determinism checkbox in Emitter node > Selection panel > Emitter Properties > Emitter.


# References

- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
