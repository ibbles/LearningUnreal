The [[Asset Editor]] for [[Unreal Motion Graphics - UMG]] widgets.
The Widget Editor contains a number of parts split over two modes.
The Designer mode is where we design the content of our widget.
The Graph mode is where we implement the behavior of our widget.
The Graph mode is very similar to other [[Node Graph Editor]] based [[Asset Editor]]s.

- Designer mode
	- Designer panel
		- The grid in the middle where we can see what our widgets looks like.
		- Also called the viewport.
	- Palette panel
		- A library of widgets that can be used to build up our widget.
	- Hierarchy panel
		- Hierarchy of sub-widgets that build up our widget.
	- Details panel
		- Settings for the currently selected object.
- Graph mode
	- My Blueprint panel
		- Contains a list of functions, variables, event dispatchers added to our widget.
	- Details panel
		- Shows the properties of the currently selected object.
	- Event Graph
		- [[Node Graph Editor]] where we define how the widget should respond to events.

# Designer Mode
## Palette Panel

Lists widget types that can be added to our widget.
Unreal Engine comes with a bunch of widget types.
Such as
- Button
- Check Box
- Image
- Progress Bar
- Text
- List View
- Canvas Panel
Widgets that are part of the project are listed  under User Created.

## Hierarchy Panel

Contains a tree of sub-widgets that we have added from the Palette panel.
The root of the tree is the widget itself.
The structure of the hierarchy define how sub-widgets are nested within each other.
Delete widgets by selecting them and hitting the Delete key.
Deleting a container widget will delete the child widgets as well.

## Details Panel

Works just like any other [[Details Panel]].
Shows the properties of the widget currently selected in the Hierarchy panel.

## Designer Panel

There is a tool bar at the top of the Designer panel.
Use the Screen Size drop-down to select the screen size used to preview your widget.
The widget will work on other screen sizes as well, if set up correctly.
The bottom-left corner shows some information about the currently selected preview size.
Delete widgets by selecting them and hitting the Delete key.

## Adding Sub-Widgets

Add a sub-widget by dragging it from the Palette panel to either the Hierarchy panel or the Designer panel.
If dragged to the Hierarchy panel, the new widget will become a child of the widget it is dropped on top of,
or a sibling of the widget it is dropped next to.

The sub-widget can be moved around in the Designer panel.
Unless it was placed inside a container widget that controls its position. (I think.)


# Graph Mode

Works very much like any other [[Blueprint Class Editor]].


## Events

A widget has some events, such as Tick, that we recognize from other types of [[Blueprint Class]]es,
and some events, such as Event Construct, that are specific to widgets.

- Pre Construct
	- Called at design time.
	- Similar to [[Construction Script]] in [[Blueprint Class]].
	- Useful when you have multiple related widgets that should respond to changes made in each other.
- Tick
	- Called every frame.

It is tempting to update HUD elements in Tick,
reading data from the [[Player State]] and formatting strings and setting progress bar values.
But this is inefficient since most update would write the same value again and again.
It is better to use custom events and only update the UI when necessary.


## Custom Event

A [[Custom Event]] is one that we trigger ourselves from our game logic in another [[Blueprint Class]],
or bind to a [[Delegate]].
An example Custom Event may be named Update Health.
A Custom Event can take an input, for example the new health.
With the Custom Event selected click [[Details Panel]] > Inputs > + button.
The input needs a name and a type.
To trigger the [[Custom Event]] you need a reference to the widget instance.
The reference is available as an output pin on the Create Widget node.
Save this reference in a variable and use the variable to trigger the [[Custom Event]].


# Manipulating Sub-Widgets

Many widget types have function that we can call to either inspect or manipulate the state of the widget.
For example, the Text widget has the Set Text function.
To call a function on a widget drag a reference to the widget from the My Blueprint panel > Variables list.
If your widget isn't listed under variables then make sure the Designer mode > your widget > Details panel > Is Variable check box is ticked,
and compile the Blueprint.

You may do any kind of logic between the Custom Event node's output pins, i.e. the inputs to the [[Event]], and the input pins on the nodes for the functions you call on the widget.


# References

- [_Your First Hour in UMG_ by Epic Games, Matthew Wadstein 2019 UE4.24](https://dev.epicgames.com/community/learning/courses/l3p/unreal-engine-your-first-hour-in-umg/6o5/introduction)

