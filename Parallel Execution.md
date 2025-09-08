See also [[Async]].


# Performance Measurement

When running code multi-threaded for performance reasons it is important to measure and compare the runtime of the final application to ensure we aren't adding needless complexity for a very small performance gain.
This can be done quickly by printing the wall clock time for both the serial and parallel implementation.


```cpp
void work()
{
	const double StartTime = FPlatformTime::Seconds();

	ParallelFor(...);

	const double EndTime = FPlatformTime::Seconds();
	const double Duration = (EndTime - StartTime) * 1000.0;
	UE_LOG(LogTemp, Warning, TEXT("OffsetRocks took %f ms."), Duration);
}
```


We can also integrate with [[Unreal Insights]] using `TRACE_CPUPROFILER_EVENT_SCOPE(Name);`:
```cpp
void work()
{
	TRACE_CPUPROFILER_EVENT_SCOPE(Name);
	ParallelFor(...);
}
```

It is recommended to not put `TRACE_CPUPROFILER_EVENT_SCOPE(Name);` inside `ParallelFor` since it comes with some overhead and each iteration is potentially very short.


# `ParallelFor`

Unreal Engine has a task graph on which for loop iterations can be executed in parallel.

```cpp
#include "Async/ParallelFor.h"


void work(TArray<double> Data)
{
	ParallelFor(Data.Num(), [&Data](int32 Index)
	{
		// Do work on Data[Index].
	}, false);
}
```

The last parameter, `false` in the example, is a flag to force single-threaded execution.
Useful for debugging, testing if a bug is caused by the parallel execution, and as a quick switch when measuring parallel speed-up though for that purpose I would recommend not using `ParallelFor` at all.

# References

- 1: [_Task System_ @ dev.epicgames.com/documentation UE 5.5](https://dev.epicgames.com/documentation/en-us/unreal-engine/tasks-systems-in-unreal-engine)
- 2 : [_Task Graph Insights_ @ dev.epicgames.com/documentation UE 5.5](https://dev.epicgames.com/documentation/en-us/unreal-engine/task-graph-insights-in-unreal-engine-5)
