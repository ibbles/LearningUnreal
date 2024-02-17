# Component To Owning Actor

A Component can notify the owning [[Actor]] about events through a  [[C++ Interface]].
The intention is that an [[Actor Component]] type and an [[Actor]] type is made for each other by having the [[Actor]] implement a [[C++ Interface]] that the [[Component]] calls whenever some interesting thing happens.
```cpp
void UMyComponent::MyThingHappened()
{
	IMyInterface* MyInterface = Cast<IMyInterface>(GetOwner());
	if (MyInterface != nullptr)
	{
		MyInterface->Execute_MyThingCallback(/*Owner here?*/);
	}
}
```