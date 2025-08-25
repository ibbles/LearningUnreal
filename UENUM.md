A preprocessor macro parsed by [[Unreal Build Tool]] to expose a C++ [[Enum]] to [[Blueprint]].

```cpp
#include "UObject/ObjectMacros.h"

UENUM()
enum EMyUnscopedEnum : uint8
{
    FirstMember,
    SecondMember
};

UENUM()
enum class EMyScopedEnum : uint8
{
    FirstMember,
    SecondMember
};

void Demo()
{
    std::cout << "Accessing an unscoped enum: " << FirstMember;
    std::cout << "Accessing a scoped enum: " << EMyScopedEnum::FirstMember;
}
```

