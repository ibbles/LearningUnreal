# Blueprint Functional Tests

A Blueprint Functional Test is a [[Level]] with a name starting with `FTEST_` along with a Blueprint instance in that level created from a Blueprint inheriting from the Functional Test base class.
The responsibility of the Functional Test Blueprint is to observe the world and report any bad behavior.
This is often done by having an instance editable [[Blueprint Variable]] that references some object to be observed.
The same reference can also be used to control that object, for example to make a [[Character]] walk, shoot, or take any other action.

The Functional Test has a number of [[Blueprint Event|Blueprint Events]] it can listen to:
- Begin Play.
	- The same [[Begin Play]] as for any other [[Actor]].
- Prepare Test.
	- Called to signal that the test is about to start. After this the virtual `IsRead` function will be called every tick until it returns true.
- Start Test.
	- Actual test code goes here.

The execution rooted at Start Test should, at some point, reach a Finish Test node.
This is what tells the unit test framework whether the test passed or failed,
unless you used any of the Assert functions that failed then that takes precedence.
It does not have to call Finish Test immediately,
it is OK to use a [[Blueprint Timer]], a [[Delay]] node, or any other asynchronous engine feature to evaluate the test condition in some future tick.

Beware that Prepare Test is not necessarily called at the first frame,
the simulation may progress some time before that event is triggered.
If you need to inspect the initial state or do some setup before the simulation starts the use [[Begin Play]].
Beware that the order that [[Begin Play]] is called for different Actors is unpredictable,
you cannot assume that some Actor you wish to inspect or modify has been initialized already.
One way to handle this is to use a Is Initialized flag and an [[Event Dispatcher]] in the other class.
On [[Begin Play]] that class initializes itself, sets the flag, and triggers the [[Event Dispatcher]].
On [[Begin Play]] the Functional Test checks the flag on the other class and if set does whatever needs to be done.
If not set then the Functional Test registers itself with the other class's [[Event Dispatcher]] and does the work in the [[Custom Event]] instead.

It is often a good idea to set a time limit at [[Class Defaults]] > Details > Timeout > Time Limit.

Blueprint Functional Tests are run from the [[Session Frontend]].
The Session Frontend is opened with [[Top Menu Bar]] > Tools > Session Frontend.
Switch to the Automation Tab.
Your `FTEST` levels will show up under Project > Functional Tests > Tests.
If you modify the set of Functional Test instances in the [[Level]] then click the Refresh Tests button in the [[Session Frontend]] tool bar.

If there are multiple Functional Tests within the scene then they are run one after the other within the same session.
Meaning changes made and time passed during the first test is seen by the second test.
I don't yet know how to control the order of the tests.

When the test is run a new [[Viewport]] will open where we can see what is happening in the scene.
The camera used to view the scene can be set on the Functional Test instance in our level, at [[Details Panel]] > Functional Testing > Observation Point.
Create a [[Camera]] [[Actor]], place it where you want it, and assign that to Observation point on the Functional Test instance.


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
	-NoSound -Unattended -NullRHI -NoSplash
```

The results of the tests are written to `index.json` in the path passed to `-ReportOutputPath`, along with a bunch of log files.
`index.html` is also generated in the same directory, but I have never seen that render anything, it's just a blank white page.


# References

- [Jenkins, CI and Test-Driven Development](https://unrealcommunity.wiki/jenkins-ci-amp-test-driven-development-6912tx0c)
- [Considerations On Testing UE4 Classes Part I: Worlds, Ticks and Forces](https://unrealcommunity.wiki/considerations-on-testing-ue4-classes-y45ygvk0)
- [Considerations On Testing UE4 Classes Part II: Editor clicks and Gamepad/Keyboard button presses](https://unrealcommunity.wiki/considerations-on-testing-ue4-classes:-part-ii-8b09e4)
- [Considerations On Testing UE4 Classes Part III: Replication](https://unrealcommunity.wiki/considerations-on-testing-ue4-classes:-part-iii-replication-2d68d4)

 