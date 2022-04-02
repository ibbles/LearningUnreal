An Actor Component, often just Component, is something that is added to an Actor to provide some functionality.
A Component may have a transformation or not.
Components with a transformation inherit from Scene Component.
Scene Components exist in a hierarchy within the owning Actor.
When a Scene Component is transformed all its children will transform with it.
[[Transform]]

Components can be destroyed by calling the Destroy Component member function.
Until the end of the frame (I think) the Component will still exist but the Is Component Being Destroyed member function will return `true`.
(
I'm not sure the above is true.
)
I'm not sure what happens if you access a Actor member Component after it has been destroyed.
Is it still safe to call Is Valid on it?
