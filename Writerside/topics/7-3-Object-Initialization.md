# 7.3. Object Initialization

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Object Initialization

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

The `sup=` argument must be provided if the class has any stateful superclasses. These arguments must be provided in a
tuple of all required superclasses.

```
@inherit[Copy, Clone]
cls MyType {
    x: BigInt
    y: BigInt
}

cls Other {
    z: BigInt
}

let o = Other(z=3)
let obj = MyType(x=1, y=2, sup=(o,))
```

Because `Copy` and `Clone` are stateless classes (no attributes), they do not need to be provided in the `sup=`
argument, as they don't affect the memory layout of the object.
