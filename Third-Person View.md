A third-person view is created by positioning a [[Camera]] some distance away from the [[Static Mesh|Static]] or [[Skeletal Mesh Asset|Skeletal]]  mesh that is being controlled.
It is common to make the [[Camera Component]] an attachment-child to a [[Spring Arm Component]].

A Spring Arm provides a bit of dynamics to the motion of the camera relative to the rest of the Actor and can react to collisions.
Control whether or not collision detection should be performed with Details Panel > Camera Collision > Do Collision Test.

The Sprint Arm can be rotated around the Actor.
We can set a length of the Spring Arm.
The position and orientation of the Camera attached to the Sprint Arm follows when we change the settings on the Sprint Arm.

If we want the Camera to face the same direction even when the Actor rotates, which is common in top-down views, then the Spring Arm's rotation can be set to world/absolute instead of relative.
