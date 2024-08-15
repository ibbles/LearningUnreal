Blueprint Reconstruction is the process of serializing all Components in a [[Blueprint Actor]] instance, deleting them, and the recreating them from the [[Blueprint Class]] definitions.
This happens whenever we change a [[Blueprint Actor]] instance, including during [[Play In Editor]] sessions.
Any Component added with Add TYPE Component will be deleted and not recreated.
