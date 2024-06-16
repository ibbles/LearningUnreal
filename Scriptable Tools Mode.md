The Scriptable Tools Mode is an [[Editor Mode]] that contains interactive tool that we can implement ourselves.
Implemented in a pair of bundled [[Plugin]]s named Scriptable Tools Editor Mode and Scriptable Tools Framework.
Each individual tool is implemented as a [[Blueprint Class]].

The Scriptable Tools Mode provide two panels:
- Tool Palette panel
	- A list of tools we have access to.
- Tool Properties panel
	- Properties for the selected tool.

When a tool is clicked in the Tool Palette panel it becomes activated and its properties, if any, are shown in the Tool Properties panel.
At the bottom of the [[Level Viewport]] a set of buttons will appear, either Complete or Accept / Cancel.

These tools are editor-only, cannot be used in a packaged application.
Build upon the interactive tools framework.
Gives access to mouse input, gizmos, accept / cancel buttons, a tools settings panel, and more.
A user-defined tool is created by adding a [[Blueprint Class]] that inherits from one of:
- Editor Scriptable Interactive Tool
- Editor Scriptable Single Click Tool
- Editor Scriptable Click Drag Tool

(
What should I do if I need both click events and drag events?
)

Tool settings are defined in a Property Set.
See _Tool Property Set_ below.

The tools can add simple visualizations to the [[Level Viewport]], such as points, lines, and text.


# Creating A Tool

[[Content Browser]] > right-click > _Create Advanced Asset_ > Editor Utilities > Editor Utility Blueprint.
As the parent class pick one of
- Editor Scriptable Interactive Tool
- Editor Scriptable Single Click Tool
- Editor Scriptable Click Drag Tool

Typically named with a `BP_Tool_` prefix.

Edited in the regular [[Blueprint Class Editor]].

[[My Blueprint Panel]] has been extended with a Scriptable Tool Settings category.
Contains [[Blueprint Variable]]s that should be filled in.
May need to enable [[My Blueprint Panel]] > top-right gear > Show Inherited Variables.
These can also be set from [[Class Defaults]] > [[Details Panel]] > Scriptable Tool Settings.
- Tool Name: The name that will be shown in the Tool Palette panel of the Scriptable Tools [[Editor Mode]].
- Long Name:
- Category: The category that the tool will show up in in the Tool Palette panel.
- Tooltip:
- Visible In Editor:
- Shutdown Type: Whether the tool should show the Complete button or the Accept / Cancel pair of buttons.


# Events / Overridable Functions

There are a number of virtual functions we can override.
Child classes of Editor Scriptable Tool may provide additional functions.
Override a function by either:
- [[My Blueprint Panel]] > top-left > Add > Override Function.
- [[My Blueprint Panel]] > right-click > Override Function.
- [[My Blueprint Panel]] > Functions > Override drop-down.

The overridable functions are:
- On Script Setup: Called when the tool is selected in the Tool Palette panel.
	- Used to setup a Tool Property Set and create preview objects.
- On Script Can Accept.
- On Script Tick: Just a regular [[Tick]].
- On Script Render. Used to draw 3D graphics.
	- Draw any visualization the user needs to use the tool.
- On Script Draw HUD: Used to draw 2D graphics.
- On Gizmo Transform Changed.
	- Called when one of the gizmos created with Create TRS Gizmo function is moved by the user.
- On Script Shutdown.
	- Called when the tool "ends", either because it was unselected (I assume.), the user clicked the Accept button, or the user clicked the Cancel button.


## On Script Setup

An overridable function that is run when the tool is selected in the Tool Palette panel.
If the tool has a Tool Property Set then this is where we should call Add Property Set Of Type and Restore Property Set Settings.
If the tool uses a [[Transform Gizmo]] then it can be created here.
See _Transform Gizmo_ below.

	- If you have a Scriptable Interactive Tool Property Set child class, call Add Property Set Of Type, promote it to a [[Blueprint Variable]], 

## On Script Render

Called once per frame.
Provides a Render API output which is an object that provides rendering functions under Scriptable Tool > Render when dragging off of the pin.

The Draw Line function is used to draw lines.
Give it two points in world space and a line will be drawn between them.
I assume there are other rendering primitives as well.

The Draw functions return the render object to make it easier to chain multiple draw calls.


## On Script Draw HUD

Called once per frame.
Provides a Draw HUD API output which is an object that provides widget functions under Scriptable Tool > Draw HUD.

The Draw Text At Location draws text at a world location.
Can for example be used to show the name of the selected [[Actor]]'s display name next to it.
Get the selected [[Actor]] with Get Selected Actors.

## On Script Shutdown

Called when the user clicks one of the Completed, Accept, and Cancel button.
(
Is it also called if the user switches to another tool?
)
Also called if the use switches to another [[Editor Mode]], and when the tool itself requests a shutdown with the Request Too Shutdown function.

A [[Blueprint Event]] (Or overridable function? Not sure.) that the Editor Scriptable Tool can listen for.
Add to the [[Event Graph]] with [[My Blueprint Panel]] > Add > Override Function > On Script Shutdown.
Has Shutdown Type output pin, one of Completed, Accept, Cancel.
These corresponds to the buttons at the bottom of the [[Level Viewport]] when the tool is active.
Use a Switch On Tool Shutdown Type node to branch based on the shutdown type.

If you have a Property Set child class with Instance Editable [[Blueprint Variable]]s it is common to call Save Property Set Settings on Completed and Accept.
That makes it so that the next time the tool's On Script Setup logic is run the Restore Property Set Settings call will get those values back.


# Tool Property Set

A tool may provide settings to the user.
The settings are shown in the right half of the Tool Palette panel.

## Creating A Property Set

There is [[Blueprint Class]] named Scriptable Interactive Tool Property Set.
Inherit from this to create properties for the Editor Scriptable Interactive Tool [[Blueprint Class]] child class.

## Adding Properties To A Property Set

Variables in this class that are marked Instance Editable will appear in the property window, the right half of the Tool Palette panel.

## Associate With A Tool

To associate a Property Set with a Scriptable Tool, in the Scriptable Tool's [[Event Graph]], on the On Script Setup event, call Add Property Set Of Type and in the Property Set Type drop-down select your Property Set subclass.
Promote the New Property Set output to a [[Blueprint Variable]].
Casting may be required to make the variable have the actual type of your particular settings rather than a general Tool Property Set.
Having the correct type makes it easier to access the properties it holds.

I think Property Sets can be added after On Script Setup as well.

## Read Property Values

To read a property from the Tool Property Set call the Get Editor Property function.
Connect your Property Set variable, created from the output of the Add Property Set Of Type node, to the Object input on the Get Editor Property node.
The Property Name should be the name of an Instance Editable [[Blueprint Variable]] in the Scriptable Interactive Tool Property Set child class.

If you gave the [[Blueprint Variable]] the exact type of your Tool Property Set child class then you may be able to access the variable directly on a variable reference node, just like any other Blueprint member.

## Property Changed Callback

To be notified when a property changes to can add an event with the Watch Property function.
This takes an [[Blueprint Event]] input named On Modified.
Drag off of this input pin and select Create Custom Event.
The [[Custom Event]] will receive the property set that was changed and the name of the property within that set that was changed.
There are typed overloads such as Watch Bool Property and Watch Int Property.
I assume the include the new value with the event.

## Store / Restore Property Values

To have the tool remember settings between uses, call Restore Property Set Settings in the On Script Setup event.
In the On Script Shutdown event call Save Property Set Settings if Shutdown Type is Completed or Accept.
That makes it so that the next time the tool's On Script Setup logic is run the Restore Property Set Settings call will get those values back.

Settings are stored per Property Set, not per tool, which means that if multiple tools use the same Property Set type then they will share saved settings.
If you don't want this then pass a tool-unique Save Key to the Save Property Set Settings and Restore Property Set Settings.

## Controlling Property Visibility

A property may not always be relevant.
For example enabling one setting may cause other related settings to appear.
A property is shown or hidden with the Set Property Visible By Name function.
This is often done from the property changed callback of the property that has related settings, and possibly also On Script setup for the initial state.

It is also possible to show and hide an entire Property Set using Set Property  Set Visible By Name.


# Transform Gizmo

## Create Gizmo

Create a [[Transform Gizmo]] by calling Create TRS Gizmo on the On Script Setup event.
I think TRS stands for Transform Rotate Scale.
I think gizmos can be created in other events as well.
The Gizmo Options input let you control what features should be enabled on the [[Transform Gizmo]], such as if translation, rotation, scale, or all should be allowed, which axis / planes should be free, what coordinate system it should use, and if snapping should be enabled.
Give the gizmo a unique identifier.

After the gizmo has been created we can get its transform using the Get Gizmo Transform function.

When the gizmo is moved the new transform is passed to the Gizmo Transform Changed event.

## On Gizmo Transform Changed

Override the On Gizmo Transform Changed function with [[My Blueprint Panel]] > Add > Override Function > On Gizmo Transform Changed.
A new [[Blueprint Event]] node named Event On Gizmo Transform Changed will show up in the [[Event Graph]].
Will have an output pin named Gizmo Identifier that is the identifier given to Create TRS Gizmo.
Useful if our tool uses multiple gizmos.
Also has New Transform output pin.

## Destroy Gizmo

If you no longer need a gizmo then it can be destroyed with the Destroy TRS Gizmo function.
You don't need to do this a tool shutdown, all gizmos are destroyed automatically at that point.


# Editor Scriptable Single Click Tool

In addition to the Scriptable Tool Settings category, this [[Blueprint Class]] also has the Single Click Tool Settings category in the Variables category in the [[My Blueprint Panel]].
Contains the Want Mouse Hover [[Blueprint Variable]].
(
Don't know what this does yet.
I assume it must be enabled to get the hover events described below.
I assume it is a performance optimization to turn this off.
)

Has a number of overridable functions in addition to the basic ones.
- On Hit By Click: Called when the user clicks in the [[Level Viewport]].
- On Hover Begin:
- On Hover End:
- On Hover Hit Test:
- On Hover Update:
- Test If Hit By Click: Determine if a user click is relevant for this tool.

## On Hit By Click

[[Blueprint Event]] that is triggered when the user clicks in the [[Level Viewport]].
The Event node has Click Pos and Modifiers output pins.
Click Pos is not a position but an Input Device Ray, which has both World Ray and Screen Position.
World Ray, in turn, has Origin and Direction.
Origin is the camera position. (I assume.)
Direction is the direction from the camera through the clicked coordinate on the projection plane.
This information can be used to perform a [[Line Trace]] to find what was clicked.
Connect Click Pos > World Ray > Origin to the Start input.
Connect Origin + (Direction times some ray length as a float) to the End input.
The ray length to use depends on the project and contents of the level.
You can right-click the other input pin of the multiply node and select To Float.
Too large is often a better first guess than too small, so that we don't frustrate our users with clicks that don't register because our line trace ray is too short..

The Hit Result we get back from the [[Line Trace]] can, for example, be used to [[Spawn]] an [[Actor]] at that location with Spawn Actor From Class.


## On Hover Begin / Update / End


# Editor Scriptable Click Drag Tool

Parent class for tools that work with mouse drag gestures.
A drag consists of four steps, each implemented as an overridable function:
- Test If Can Begin Click Drag
- On Drag Begin
- On Drag Update Position
- On Drag End

## Test If Can Begin Click Drag

This function should return an Input Ray describing where the drag starts.
Use Make Input Ray Hit if a start point can be determined.
Use Make Input Ray Hit Miss if there is no valid start point for the click location.

## On Drag Begin

Only called if Test If Can Begin Click Drag returned a valid Input Ray Hit.
Here we can do Property Set value validation and initialize any visualization rendering we may need to do during the drag.

## On Drag Update Position

Called every time the mouse is moved while a drag is in progress.


## On Drag End

Called when the user releases the mouse button.
Here is were we would realize whatever effect it is that the tool should do.


# Error Reporting

Message functionality is available under [[Node Graph Editor]] > right-click > Scriptable Tools > Messaging.

The Display User (Help|Warning) Message adds a help or warning message.
Help messages goes to the status bar at the bottom of the Unreal Editor window.
Warning messages toes to top of the tool's settings panel.
This message must be explicitly removed when it is no longer relevant.

Messages are removed with Clear User Messages.
Can chose to clear help messages, warning messages, or both.

Use Add Log Message to write to the log instead of to the UI.

# /

The Get Selected Actors node is used to get access to the [[Actor]]s that are currently selected in the [[Level Viewport]].

The Get Tool World function gives access to the [[World]] in which the tool is active.
In most (every?) cases this will be the world shown in the [[Level Viewport]].
This can be either the [[Level]] [[Asset]]'s world, or a [[Play In Editor]], i.e. temporary, world.


# References

- [_Scriptable Tools & Editor Mode Reference_ by Michael Munir @ dev.epicgames.com/tutorials 2023 UE5.2](https://dev.epicgames.com/community/learning/tutorials/7BKd/unreal-engine-scriptable-tools-editor-mode-reference)
- [_Scriptable Tools & Editor Mode_ by Michael Muni @ dev.epicgames.com/tutorials 2023 UE5.2](https://dev.epicgames.com/community/learning/tutorials/1loo/unreal-engine-scriptable-tools-editor-mode)
- [_Editor Scriptable Tools: Basic Tools UE5.2_ by Volkiller Games @ youtube.com 2023](https://www.youtube.com/watch?v=xWxtxS2jADo)
- [_UE5.2 Scriptable Tools : Node overview_ by Volkiller Games @ youtuve.com 2024](https://www.youtube.com/watch?v=w-PXmo8QKZs)
- [_Unreal Engine 5.2 - Scriptable Tools Introduction (Physics Scattering)_ by renderBucet @ youtube.com 19:43 2023](https://www.youtube.com/watch?v=0-DcPwR4CI0)
- [_Boosting your productivity with Scriptable Tools and PCG_ by Alban Bergeret @ dev.epicgames.com/tutorials 2023 UE5.3](https://dev.epicgames.com/community/learning/tutorials/5G6b/unreal-engine-boosting-your-productivity-with-scriptable-tools-and-pcg)
