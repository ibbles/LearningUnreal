A tick is when the game state moves from one state to the next, followed by a frame render.
By default Unreal Engine ticks as fast as it can.
This means that the time passed between consecutive ticks may vary over time.
The tick rate is also called the [[Frame Rate]], though the two aren't exactly the same since ticking happens on the [[Game Thread]] and rendering on the [[Render Thread]].
You can set a target frame rate in [[Project Settings]] > Engine > General Settings > Framerate > Fixed Frame Rate.
This is just a target though, if the machine isn't fast enough then the frame/tick rate will be lower than the target.


# Tick Event

Some types, such as [[Actor]] and [[Component]], can get a callback once per tick, or some other interval / tick frequency.
The callback is named `Tick`.
In C++ the virtual member function is called `Tick` or `TickComponent`, depending on the parent class.

For a [[Blueprint Class]] the callback interval is set at [[Blueprint Editor]] > [[Class Defaults]] > Actor Tick > Tick Interval.

The `Tick` callback is passed the amount of time that has passed since the last tick, called `Delta Time`.
Or rather, the time the previous tick took in total.
The sum of all `Delta Time` is the total game time.
(
Is this true even if the callback frequency is lower than once per tick?
Is delta time the time since the last time the callback was called, or the time of the last tick?
)

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

# Tick Group


# Tick Order

Tick prerequisites are used to control the tick order within a Tick Group.
To make an [[Actor]] tick before another [[Actor]] use the Add Tick Prerequisite Actor function.
To make an [[Actor Component]] tick before another [[Actor Component]] use the Add Tick Prerequisite Component function.
I'm not sure if it is possible to create cross-prerequisites, i.e. have an [[Actor]] tick after an [[Actor Component]] or vice versa.

If the tick calls aren't made in the order you expect then set the`tick.LogTicks` [[Console Variable]] to 1.
This will print a trace of the tick calls to the [[Output Log]], together with information about the prerequisites.

# Callback On Next Tick

We can use the [[Timer Manager]] to get a callback on the next tick.

```cpp
void UMyClass::SomeFunction()
{
	TWeakObjectPtr<UMyClass> WeakThis = this;
	World->GetTimerManager().SetTimerForNextTick(
	    FTimerDelegate::CreateWeakLambda(this, [WeakThis]()
	    {
	        if(!WeakThis.IsValid())
	            return;
	        GEngine->AddOnScreenDebugMessage(-1, 5.f, FColor::Green, TEXT("It is now the next tick."));
	    }));
	}
}
```

# References

- [_Actor Ticking_ by Epic Games @ docs.unrealengine.com](https://docs.unrealengine.com/5.2/en-US/actor-ticking-in-unreal-engine/)
- [_How do I make a Spline Mesh write pixel velocities to prevent ghosting/smearing?_ by Martin Nilsson, Jon Cain, Josie Yang](https://udn.unrealengine.com/s/question/0D54z000096fwr3CAA/how-do-i-make-a-spline-mesh-write-pixel-velocities-to-prevent-ghostingsmearing)
