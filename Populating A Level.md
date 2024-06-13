# Keep Simulation Changes For Physics-Based Object Placement

If you want to have a [[Static Mesh]] fall into place physically but not have it be simulated at runtime you can do the following:
- Add a high-quality [[Collision Shape]] to the [[Static Mesh Asset]].
	- Either Use Complex As Simple or [[Convex Decomposition]].
	- This is needed for both the objects to be scattered and the objects they are falling on to.
- Place an instance of the [[Static Mesh]] in the [[Level]] some distance above the area where it should fall.
- On the instance, change [[Mobility]] to Movable.
- On the instance, enable Physics > Simulate Physics.
- Select [[Main Tool Bar]] > Play button drop-down menu > Simulate.
- Wait for the object to come to rest.
- Right-click the instance and select Keep Simulation Changes.
- Stop the simulation.
- Change the instance's [[Mobility]] back to Static.
- Remove Use Complex As Simple, or remove the [[Convex Decomposition]].

If you have many objects using many [[Static Mesh Asset]]s then:
- Use [[Content Browser]] > right-click > Asset Actions > Bulk Edit Via [[Property Matrix]] to set Static Mesh > Body Setup > Collision Complexity to Use Complex As Simple on many [[Static Mesh Asset]]s at once.
- There doesn't seem to be a way to add [[Convex Decomposition]] to multiple [[Static Mesh Asset]]s at once though.
- Select multiple [[Static Mesh Asset]]s in the [[Content Browser]] and drag into the [[Level Viewport]].
- Move them around one by one using the [[Transform Gizmo]].
- Select all the instances and Alt + LMB-drag to duplicate.
	- Create as many instances as necessary.
