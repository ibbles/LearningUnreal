Scalability is the act or increasing or decreasing the quality of an effect based on the environment in which the application is running.
By "environment" in this case we typically mean the hardware that the end-use runs the application on.
Slower hardware often necessitates lower scalability settings to reach desired performance and frame rate.

A [[Niagara Emitter]] has a Selection panel > Emitter State > Scalability > Scalability Mode.
Can be set to either System or Self.
System means that the Emitter inherits whatever scalability settings has been set on the [[Niagara System]].
Self means that we get a bunch of additional options on the Emitter itself.

A [[Niagara System]] has Selection Panel > System > Effect Type which is a reference to an [[Asset]] that contains scalability settings.
This makes it possible to configure and edit the scalability settings for many [[Niagara System]]s together.

The Niagara Effect Type contains scalability settings.
Can set draw distance and culling settings.
Can have per-platform (iOS, Android, Linux, Mac Windows, etc).
Can have a [[Console Variable]] control whether a scalability setting should be set or not.

Use [[Niagara Editor]] > Top Tool Bar > Scalability button lets you preview what the particle system looks like at various scalability settings.
A scalability selector appears in the top-left corner of the [[Node Graph Editor]].

(
Can different [[Niagara Module]]s be used for different scalability settings?
)

# References

- [_Begin Play | Niagara_ by Epic Online Learning, Arran Langmead @ dev.epicgames.com/tutorials 2023 UE5.0](https://dev.epicgames.com/community/learning/tutorials/j9YO/unreal-engine-begin-play-niagara)
