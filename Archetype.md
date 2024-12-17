An Archetype is an [[UObject]] instance that can have [[Archetype Instance]]s.
Part of the [[Blueprint]] system to connected the [[Blueprint Class]] with the instances of that [[Blueprint]] that has been created in the [[Level]].
In the [[Blueprint]] context an Archetype that is an [[Actor Component]] is also called a [[Template Component]].
The [[Blueprint Class]] contain [[SCS Node]]s that represents the [[Actor Component]]s that the [[Blueprint Class]] contains.
Each [[SCS Node]] has a [[Template Component]] that holds the default values for all [[Property|Properties]] that the [[Actor Component]] has.