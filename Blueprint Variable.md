A [[Blueprint Class]] may contain variables.
The variables are listed in the Variables category of the My Blueprint panel in the Blueprint Editor.
Variables inherited from the parent Blueprint, if any, can be included by enabling My Blueprint > Eye (UE4) or Gear (UE5) > Show Inherited Variables.
A Blueprint variable has a type that is one of the [[Blueprint Datatypes]].

Create a **new variable** by clicking the `+` next to Variables in the My Blueprint panel.
Or duplicate an already existing variable by selecting it and pressing Ctrl+W (UE4) or Ctrl+D (UE5).
The Blueprint should be compiled after adding a variable to make it possible to set a default value.
The variables type, default value, and my other settings are set in the [[Details Panel]] after selecting the variable.

**Use a variable** by either
- Dragging it from the Variables list into the [[Blueprint Visual Script Editor]]
  Hold Ctrl to get a read node, hold Alt to get a write node.
- By right-click in the node graph and searching for the name.

A Variable can be **Instance Editable**.
An Instance Editable variable will show up in the Details panel when an instance of the [[Blueprint Class]] is selected in the [[Level Editor]].
The Instance Editable flag is also shown as an eye next to the variable in the My Blueprint panel.
The eye is
- closed for non Instance Editable variables.
- open yellow for Instance Editable variables without a Tooltip.
- open green for Instance Editable variables with a tooltip.


A Variable can be **Private**.
Private variables can not be seen or edited by child classes.
Non-private variables are accessible by child classes.
To see inherited variables in a child class check My Blueprint panel > Eye icon > Show Inherited Variables.

A Variable has a Category.
The Category is set in the Details panel.
The Category is set when dragging a Variable into a Category in Variables category of the My Blueprint panel.
The Category is used when right-clicking in the [[Blueprint Visual Script Editor]] and in the Details panel of a selected [[Blueprint Instance]].

Variables exists in a **hierarchy of templates and instances** where values are inherited.
[[Variables Templates And Instances]]

A Blueprint Variable can be **local** to a [[Blueprint Function]] or the [[Construction Script]].
Local variables are only accessible while executing that function.
Local variables have a Category in the My Blueprint panel.
This Category is not shown when the [[Event Graph]] is open since the [[Event Graph]] cannot have local variables.
Create a new local variable by either clicking the `+` in the Local Variables Category in the My Blueprint panel or right-click an output pin and select Promote To Local Variable.
