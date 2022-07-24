Also called Bulk Edit.

A way to edit many [[Asset|Assets]] at the same time.
Select a bunch of [[Asset|Assets]] of the same type in the [[Content Browser]].
You can use an [[Asset Filter]] to recursive search through an entire folder structure.
Right-click > Asset Actions > Build Edit Via Property Matrix.
This will open a multi-object Details panel.
The left hand side of the panel holds a table with all the selected assets along the rows and some of the properties along the column.
We can sort this table with any column as key.
The right hand side of the panel holds a more traditional Details panel.
In the left hand side of the traditional Details panel is a pin column, pinned properties show up in the table.

Editing in the Details panel will modify all assets.
In the table we can edit one at the time.

A common workflow is
- Sort the table based on some criteria to group assets we want to make a change to together.
- Edit one for the cells for the first asset in the range we are to change.
- Select the edited cell.
- Hit Ctrl+C to copy the value.
- Select the corresponding cell on the next row, hold Shift, and click the last cell in the range.
- Hit Ctrl+V.

This may take a little while to process since it will apply the update to all the selected assets.
Do File > Save to save the modified assets.