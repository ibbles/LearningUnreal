
# GPU process launch failed

This is caused by the Chromium Embedded Framework, which is used for the Web Browser Plugin.
It can be disabled by passing `-NoCEF` when starting the Editor, and maybe also exported projects.

Example error message:
```
ERROR:gpu_process_host.cc(955)] GPU process launch failed: error_code=1002
WARNING:gpu_process_host.cc(1274)] The GPU process has crashed 8 time(s)
ERROR:gpu_process_host.cc(955)] GPU process launch failed: error_code=1002
WARNING:gpu_process_host.cc(1274)] The GPU process has crashed 9 time(s)
FATAL:gpu_data_manager_impl_private.cc(414)] GPU process isn't usable. Goodbye.
```


# ICU Version

```
Couldn't find a valid ICU package installed on the system.
```

This problem appeared with Unreal Engine 5.0, and was fixed in 5.1 by bundling the ICU.
This is a .NET thing.
That is needed when building C++ projects.
You can tell it to use a specific ICU version with
```bash
export DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1
export DOTNET_SYSTEM_GLOBALIZATION_APPLOCALICU="69.1"
```

You can find out which version you have with
```bash
find /usr/lib/icu/