Delay is a [[Blueprint Visual Script]] function that suspends execution of the current execution wire until the specified duration has passed.
When the time has passed execution will resume again.

A Delay node only support a single waiting execution.
If the same Delay node is executed again while there already is an execution waiting then the second call is ignored.
