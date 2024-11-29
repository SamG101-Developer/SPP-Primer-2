# 7.7. Memory Layout

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Memory Layout

A class object in S++ is a collection of attributes and methods. A class's memory is laid out in order of attribute
definition, where each attribute is the size of the type it represents (recursive for classes). This creates a problem
for subclass attribute types - for example, if an attribute is type `Base`, and a `Derived` instance (that inherits
`Base`) is assigned to the attribute, then with a basic model, the attribute will only store the `Base` part of the
`Derived` instance.

This is solved by using pointers under the hood, and changing nothing about the syntax, or semantics of the language. In
this case, if an attribute is going to be assigned a derived class at some point, it must be marked in the class
definition as such. This allows the class to store a pointer to a memory location where a derived object is stored.

```
cls Base {
    x: BigInt
}

@inherits(Base)
cls Derived {
    y: BigInt
}

cls Object {
    obj: Base
}
```

In this example, the `Object` class stored a `Base` object. The following code shows the assignment of a `Derived`
object to the `obj` attribute:

```
let obj = Object(obj=Derived(y=2))
```

This will slice the extra `y` attribute from the `Derived` object, and store the `Base` part in the `Object` instance.
This also happens when the `std::upcast` function is used, for example when passing a more derived type to a function
parameter who accepts a base type.

Any type that wants to be `downcast` as some point, must be wrapped in the compiler-known `Castable` type. This allows a
variable to be stored as a pointer under the hood and for the compiler to know that the variable can be downcast at some
point.

```
cls Base {
    x: BigInt
}

@inherits(Base)
cls Derived {
    y: BigInt
}

cls Object {
    obj: Castable[Base]
}
```

The `std::upcast` and `std::downcast` function definitions are:

```
fun upcast[Base, Derived: Base](obj: Derived) -> Base { ... }

fun upcast[Base, Derived: Base](obj: Castable[Derived]) -> Castable[Base] { ... }

fun downcast[Derived: Base, Base](obj: Castable[Base]) -> Opt[Castable[Derived]] { ... }
```

There is a non-`Castable` version of the `upcast` function, which just slices the extra attributes from the derived
object and changes the type to the base type. Note that these functions consume their arguments, and return a new object
