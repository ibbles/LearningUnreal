# Running Tests From Command Line

Unreal Editor can run unit tests in batch mode, without a GUI.
We may tell it which level to load, and then tell it to run a bunch of tests and then exit.
There are different ways to tell Unreal Editor to exit after the tests, one that worked before Unreal Engine 5.1 and one that works with Unreal Engine 5.1.
I think the command below will handle both cases.
The difference is the addition of ` ; Quit` in `-ExecCmds`.
With Unreal Engine versions prior to 5.1 it was enough to pass the `-TestExit` bit.
Any number of tests can be listed in `-TextCmds`, separate them with `+`.

```
$UE_ROOT/Engine/Binaries/Linux/UnrealEditor \
	$PROJECT_PATH/$PROJECT_NAME.uproject \
	/Game/$LEVELS_DIR/$LEVEL_NAME.umap \
	-ExecCmds="Automation RunTests $TEST_1_FULL_NAME+$TEST_2_FULL_NAME ; Quit" \
	-TestExit="Automation Test Queue Empty" \
	-ReportOutputPath=./test_report \
	-log ABSLOG=test_log.log \
	-nosound -Unattended -NullRHI -nosplash
```

The results of the tests are written to `index.json` in the path passed to `-ReportOutputPath`, along with a bunch of log files.
`index.html` is also generated in the same directory, but I have never seen that render anything, it's just a blank white page.


# References

- [Jenkins, CI and Test-Driven Development](https://unrealcommunity.wiki/jenkins-ci-amp-test-driven-development-6912tx0c)
- [Considerations On Testing UE4 Classes Part I: Worlds, Ticks and Forces](https://unrealcommunity.wiki/considerations-on-testing-ue4-classes-y45ygvk0)
- [Considerations On Testing UE4 Classes Part II: Editor clicks and Gamepad/Keyboard button presses](https://unrealcommunity.wiki/considerations-on-testing-ue4-classes:-part-ii-8b09e4)
- [Considerations On Testing UE4 Classes Part III: Replication](https://unrealcommunity.wiki/considerations-on-testing-ue4-classes:-part-iii-replication-2d68d4)

