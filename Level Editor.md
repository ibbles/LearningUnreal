The Level Editor is the main part of the [[Unreal Editor]] application.
It is what is shown when Unreal Editor is opened.
The Level Editor includes
- [[Top Menu Bar]], the one with File, Edit, Window, etc menus.
	- The [[Actor]] menu is context-sensitive and only populated when an [[Actor]] is selected in the [[Level Viewport]].
		- This menu is similar to the [[Level Viewport]] right-click menu, but have some additional actions.
	- At the end of the Top Menu Bar the project name is shown.
		- Hover over the project name to see version of the engine, graphics [[Rendering Hardware Interface]] (RHI) currently used.
- [[Tool Bar]]
- [[Level Viewport]], where we can see and modify the contents of the current level.
- [[Outliner]], where we can see a list of the [[Actor|Actors]] in the current level.
- [[Details Panel]], where we can see the [[Property|Properties]] of the currently selected [[Actor]] or [[Component]].
- [[Content Browser]], where we can see the [[Asset|Assets]] available in our [[Project Structure|Project]].
	- With Unreal Engine 5 we got the [[Content Drawer]], which is a quick-access panel containing a [[Content Browser]].
Among many other optional panels.
There is also a [[Top Menu Bar]] and a [[Tool Bar]].

When a new [[Asset Editor]] is opened it either appear as a tab within the Level Editor or as a stand-alone window.
The level editors starts with a tab for the currently open [[Level]].

# Modes

The level editor is always in one particular [[Editor Mode]].
The default mode is the [[Select Mode]], which is used when selecting, adding, or modifying the [[Actor|Actors]] of the currently open level.
Switch between the modes with the modes drop-down in the [[Tool Bar]].
Example modes:
- [[Select Mode]]
	- Called [[Place Mode]] in Unreal Engine 4.
- [[Landscape Mode]]
- [[Foliage Mode]]
- [[Modeling Mode]]
- [[Animation Mode]]

