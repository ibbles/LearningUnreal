There are a lot of string types in Unreal Engine.

```cpp
FName Name;
FString String;
std::string STDString;

TCHAR_TO_UTF8(*TheFString); // Temporary storage, only for function parameters.
TCHAR_TO_ANSI
```


```
 From\To    │FName             │FString           │FText               │char*               |UTF-8*              │TCHAR*              │std::string│
├───────────┼──────────────────┼──────────────────┼────────────────────┼────────────────────┼────────────────────┼────────────────────┼
│FName      │                  |FName::ToString   │FText::FromName     |                    |                    |*Name.ToString()    |
├───────────┼──────────────────┼──────────────────┼────────────────────┼────────────────────┼────────────────────┼────────────────────┼
│FString    │ FName(FStstring) │                  │FText::FromString   │                    |                    |                    |
├───────────┼──────────────────┼──────────────────┼────────────────────┼────────────────────┼────────────────────┼────────────────────┼
│FText      │                  |                  |                    |                    |                    |                    |
├───────────┼──────────────────┼──────────────────┼────────────────────┼────────────────────┼────────────────────┼────────────────────┼
│char*      │                  |                  |                    |UTF8_TO_TCHAR       |                    |                    |
├───────────┼──────────────────┼──────────────────┼────────────────────┼────────────────────┼────────────────────┼────────────────────┼
│UTF-8*     │                  │                  │via UTF-8*→TCHAR*   │                    │UTF8_TO_TCHAR       │                    |
├───────────┼──────────────────┼──────────────────┼────────────────────┼────────────────────┼────────────────────┼────────────────────┼
│TCHAR*     │                  │                  │FText::FromString   │TCHAR_TO_UTF8       │                    |                    |
|           |                  |                  |LOCTEXT             |                    |                    |                    |
├───────────┼──────────────────┼──────────────────┼────────────────────┼────────────────────┼────────────────────┼────────────────────┼
│std::string|                  |                  |                    |                    |                    |                    |
└───────────┴──────────────────┴──────────────────┴────────────────────┴────────────────────┴────────────────────┴────────────────────┴
```


String Handling
https://docs.unrealengine.com/4.26/en-US/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/StringHandling/

Character Encoding
https://docs.unrealengine.com/4.26/en-US/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/StringHandling/CharacterEncoding/
