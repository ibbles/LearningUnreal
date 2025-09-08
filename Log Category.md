We can create new Log Categories for use with [[Output Log]].
New log categories is created as follows, where we define a new Log Category named `MyCategory`:

`MyLogCategory.h`:
```cpp
MYMODULE_API DECLARE_LOG_CATEGORY_EXTERN(LogMyCategory, Verbose, All);
```

Source file:
```cpp
DEFINE_LOG_CATEGORY(LogMyCategory);
```

