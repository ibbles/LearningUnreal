C++ is for business logic, Blueprint for design and art logic.
Provide hooks into the business logic that the design and art logic can use.

Have a C++ base class for all gameplay objects.
It is OK for that class to have nothing but empty functions intended to be overridden with [[Blueprint Visual Script]].
This makes it possible to access the class in both C++ and Blueprints.


Benefits of C++:
- Performance.
	- For example for networking and code replication.
	- For example complex AI with a large number of actors.
- Maintenance and code readability.
	- Bigger projects with more people have it easier to maintain a large C++ code base than a large Blueprint code base.
	- The equivalent C++ code if often smaller on-screen than the Blueprint code.
	- External tools, such as diffing, revision control, web-based repository browsers support text better than Blueprints.
- Access to low-level parts of the engine.
- Can be used to create plugins.

# References

- [_Balancing Blueprint and C++ in Game Development_ by Epic Games @ dev.epicgames.com 2021](https://dev.epicgames.com/community/learning/courses/bY/unreal-engine-balancing-blueprint-and-c-in-game-development/LdK/introduction-to-the-course)
- [_Unreal advice from an ex-Epic gameplay engineer and 10 year UE user_ by u/invulse @ reddit.com](https://new.reddit.com/r/unrealengine/comments/16tnirg/unreal_advice_from_an_exepic_gameplay_engineer/)
