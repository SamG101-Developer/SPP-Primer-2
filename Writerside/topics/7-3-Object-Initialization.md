# 7.3. Object Initialization

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

<secondary-label ref="doc-subj-update"/>

## Object Initialization

Low level languages often have many different ways to initialize an object. In S++, there is a single initialization
method, called "uniform object initialization". This technique forces all the attributes of the current class to be
initialized.

No attribute must be given a value; all attributes are optional, in that they will be defaulted if not provided,
reducing noise in initializers. The order of the attributes being initialized does not matter, as they are re-arranged
in the compiler process. This is because they are all named, and as with function calls, named arguments can be in any
order.

## Uniform Object Initialization

Uniform object initialization is a simple and intuitive way to initialize objects. It follows function call syntax, but
as all types are syntactically required to be in `PascalCase`, it is clear when an initialization versus a function call
is taking place.

## Simple Uniform Object Initialization

```
cls MyType {
    x: BigInt
    y: BigInt
}

let obj = MyType(x=1, y=2)
```

In this example, the attributes `x` and `y` are given values. The order of the arguments does not matter, and the
attribute names are used as the argument names. This is the same as a function call, where the argument names are used
to match the parameters of the function.

## Defaulting Missing Attributes

With uniform object initialization, all the attributes are optional. That is, if an attribute is not given a value, it
is "defaulted". A defaulted value is an instantiation of the type, with all its attributes instantiated recursively,
with the lowest level of types begin set to `0`, `0.0`, `false`, and an empty but sized array.

```
cls MyType {
    x: BigInt
    y: BigInt
}

let obj = MyType(x=1)
```

In this example, the `y` attribute is not given a value, so it is defaulted to `0`. A more complex attribute type, such
as a class, is itself initialized as a value, recursively initializing each attribute down
to [mock-primitive](5-1-Non-Primitive-Types.md) types.

This provides a hybrid between fully initialized objects at initialization, satisfying the scope-bound resource
management principle for initialization, and not having convoluted initialization syntax, where default values are
repeated.

Whilst attributes that aren't provided an argument are automatically defaulted by the compiler, custom default values
per attribute can be assigned in the class prototype. This design was made over a `Default` type, as it allows for
simpler control to only customize the default values of certain objects.

In short, the initialization of a class in S++ is the same as a `dataclass(kw_args=True)` in Python, where attributes
can have a`field(default=...)` value, and the order of the named arguments does not matter. This is also the same as C++
without a custom constructor defined, and with named arguments.

## Base Class Initialization

When a class is initialized, any base classes are also initialized. Due to the auto-defaulting nature of attributes, the
same logic can be applied to entire superclasses; each superclass is initialized with no attributes, creating a fully
defaulted object, and inserted into memory as a base class to the current class. This means that no extra syntax is
required to initialize base classes.

```
cls Base1 {
    x: BigInt
}

cls Base2 {
    y: BigInt
}

@inherits(Base1, Base2)
cls Derived {
    z: BigInt
}

fun f() -> Void {
    let obj = Derived(x=1, y=2, z=3)
}
```

As seen above, base class members can be specified in the initializer, and are mapped onto their respective base
classes. If two base classes have the same attribute, then an error is thrown, in the same way as if there were two
conflicting function overloads.

## Entire Object Inclusion

A single `..obj` argument can be added to a class initializer, which will take the non-specified attributes of another
object of the same type, and move/copy them into the new object being made.

This will leave the `..obj` object in a partially initialized state, with the attributes moved off it into the new
object, uninitialised. All base classes will be partially initialized too, with all of their attributes moved off of
them.

```
cls MyType {
    x: BigInt
    y: BigInt
}

let m = MyType(x=1, y=2)
let n = MyType(x=5, ..m)
```

## Design Decisions

<p id="D1"><b>Attribute order</b>: Attributes can be in any order, because it is common to move attributes around in
class prototypes as a code organization technique. This would invalidate all instances of object initialization, if
argument order was fixed. Therefore, the decision was made to omit argument order checks from object initialization.
Function parameter ordering is rarely changed for code organization, so their fixed order is maintained.</p>

<p id="D2"><b>Default attributes</b>: In order to simplify memory rules, it was decided that objects must be fully
initialized when initialized, rather than potentially partially-initialized. This ties in with scope-bound resource
management. This would have led to potentially convoluted object initializers that would often have the same default
arguments (`0` for numbers etc), so the design decision was made to automatically default any non-provided attributes.
It is possible that standard defaults wouldn't be wanted all the time, such as a counter that started at `1`.
Therefore, attributes can be assigned a default value in class prototypes.

The combination of these three design decisions - full object initialization, all attributes optional, and custom
optional choices, allow for a memory-safe and simple object initialization process.
</p>

#### Default object inclusion
