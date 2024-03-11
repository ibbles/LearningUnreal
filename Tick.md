A tick is when the game state moves from one state to the next, followed by a frame render.
By default Unreal Engine ticks as fast as it can.
This means that the time passed between consecutive ticks may vary over time.
The tick rate is also called the [[Frame Rate]], though the two aren't exactly the same since ticking happens on the [[Game Thread]] and rendering on the [[Render Thread]].
You can set a target frame rate in [[Project Settings]] > Engine > General Settings > Framerate > Fixed Frame Rate.
This is just a target though, if the machine isn't fast enough then the frame/tick rate will be lower than the target.

Some types, such as [[Actor]] and [[Component]], can get a callback once per tick.
The callback is named `Tick`.
The `Tick` callback is passed the amount of time that has passed since the last tick, called `Delta Time`.
Or rather, the time the previous tick took in total.
The sum of all `Delta Time` is the total game time.

The tick callback can be done at different points during the tick.
The different points are called tick groups.
The tick groups are:
- Pre Physics
- During Physics
- Post Physics
- Post Update Work

In a Blueprint that supports the Tick [[Blueprint Event]] the tick group is set in [[Blueprint Actor Editor]] > [[Class Defaults]] > Actor Tick > Advanced > Tick Group.
To execute a [[Blueprint Visual Script]] on tick right-click the [[Event Graph]] and select Add Event > Event Tick.

In a C++ class that supports the Tick virtual member function the tick group is set at `PrimaryActorTick.TickGroup`  for [[Actor|Actors]] and `PrimaryComponentTick.TickGroup` for [[Component|Components]].
To get the tick callback override the `virtual void Tick(float DeltaTime) override` function for [[Actor|Actors]],
and `virtual void TickComponent(float DeltaTime, enum ELevelTick TickType, FActorComponentTickFunction *ThisTickFunction) override;` for [[Component|Components]].


# References

- [_Actor Ticking_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/5.2/en-US/actor-ticking-in-unreal-engine/)