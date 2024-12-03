# 7.3. Object Initialization

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Object Initialization

In C++ there is: default initialization, value initialization, zero initialization, copy initialization, static
initialization, aggregate initialization, reference initialization, constant initialization, struct initialization,
early dynamic initialization, deferred dynamic initialization, union initialization, nested initialization, direct list
initialization, copy list initialization, explicit initialization, implicit initialization, designated initialization
with different behaviour depending on where in the file a variable is declared/defined, the types of attributes on the
class, the types of methods on the class, the base classes of the class, and the template arguments of the class.

## Object Initialization in S++

Object initialization follows function call syntax almost exactly, providing a simple and intuitive way to initialize
objects:

```
cls MyType {
    x: BigInt
    y: BigInt
}

let obj = MyType(x=1, y=2)
```

Every attribute must be given a value, and the order of the arguments does not matter. If a variable has the same name
as an attribute, then the shorthand syntax can be used for that argument:

```
let y = 2

let obj = MyType(x=1, y)
```

This is equivalent to:

```
let obj = MyType(x=1, y=y)
```

## Default object

If not all attributes are given a value, then the `else=` argument can be used to provide another object of the same
type, whose attributes are moved into the new object:

```
let m = MyType(x=1, y=2)
let n = MyType(x=5, else=m)
```

This is equivalent to:

```
let n = MyType(x=5, y=m.n)
```

Therefore, `m` will be left in a partially moved state, as only the attributes not provided to the new object will
remain in `m`.

## Superclasses

C++ provides a mechanism to initialize base classes, by calling the base classes after the `:` token in the constructor.
As S++ doesn't have constructors, the base classes could either be provided in the object initialization, or in the
corresponding `sup` block that extends the superclass.

Because putting it in the initialization would cause a lot of duplication that couldn't be edited from one place, the
`sup-ext` block must contain the superclass initialization, via a special method. Only superclasses that contain state
can have initialization steps. The following example shows how `BaseClass` is initialized for `DerivedClass`:

```
cls BaseClass {
    x: BigInt
}

cls DerivedClass {
    y: BigInt
}

sup BaseClass {
    fun f() -> Void { }
}


sup DerivedClass ext BaseClass {
    # Initialization of BaseClass via method
    
    fun sup { ... }
    
    fun f() -> Void { }
}
```
