We can make our C++ code accessible to [[Blueprint Class|Blueprint Classes]] and [[Blueprint Visual Script]].
There are different ways to do this:
- [[UFUNCTION]]
- [[UCLASS]]
- [[USTRUCT]]
- [[UPROPERTY]]
- [[Blueprint Function Library]]
- [[Event]] and [[Delegate]]

The pattern is to implement base classes and functionality in C++ and then use Blueprints for customization and gameplay logic.

The opposite direction, exposing Blueprint functionality to C++, is more difficult.
