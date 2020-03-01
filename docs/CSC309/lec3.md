1. `__proto__` is property of an **object** that **points** to the object's prototype. Just normal delegate objects.
2. `prototoype` is the property of a **function** that is used as the prototype to add to the new object
when that function is called as a constructor. Every function has a prototype. <mark>Its only purpose is for when its
used as a constructor.</mark>

* __proto__ is the actual object that is used in the lookup chain to resolve methods,
* prototype is the object that is used to build __proto__ when you create an
object with new. Prototype is not available on the instances themselves
(or other objects), but only on the constructor functions.

![](prototypes_and_proto.png)