The Details panel is where the [[Property|Properties]] of an Unreal Engine object can be viewed and edited.
The contents of the Details panel will be different depending on what type of object is or objects are selected.
The contents of the Details panel for a particular type can be configured with [[Property Visibility And Editability]], [[Details Customization]], and [[Property Customization]].
Many different types of objects have a Details panel; [[Actor]], [[Actor Component]], [[Material]], [[Static Mesh Asset]], [[Skeletal Mesh]], [[Blueprint Function]], and many, many more.

The [[Level Editor]] contains a Details panel that shows the [[Property|Properties]] of the the object selected in either the [[Level Viewport]] or the [[Outliner]].
When editing from the [[Level Editor]] Details panel we edit the selected instance, not the type from which the instance was created.
Only the selected object(s) is(are) changed, nothing else.

The Details panel is organized into two columns and the rows are grouped into sections called categories.
The rows can be groups containing multiple sub-elements.
For example an array with multiple elements or a [[Struct]] with multiple members.
Click the triangle to the left to open or close such groups.
Hold Shift when clicking to open or close entire subtrees.

The Details panel can filter [[Property|Properties]] being shown.
For example by category or whether or not the property has been modified on the selected instance.
Use the buttons at the top of the to filter based on category.
Enable top-right corner > gear icon > Show Only Modified Properties to see the modified properties.


We can have multiple Details panels open at once.
Open additional ones from [[Top Menu Bar]] > Window > Details > Details [1-4].
By default all open Details panels in the [[Level Editor]] will show the [[Property|Properties]] for the same object.
A particular Details panel can be locked to continue to show the current object even when the selection is changed.
In this way we can have different Details panel showing the [[Property|Properties]] for different objects.
Lock  a Details panel by clicking the padlock icon in the top-right.

