# 11.2. Ownership Tracking

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

## Ownership Tracking

Ownership tracking is a concept in the S++ memory model that ensures only fully-initialized objects are moved, or
borrowed from. This mitigates the common memory "use-after-free", "double-free" and "uninitialized use" errors.

If a variable is in any initialization state (fully-/partially-/non-initialized), it can have a fully-initialized value
assigned to it. This makes the variable fully-initialized, and it can be moved or borrowed from. Whilst attributes can
be set, or moved off of, a fully or partially initialized object, they cannot be individually set to a non-initialized
object, the same as in Rust.

The initialization state of a variable is tracked at the symbol level, and during semantic analysis, any violations of
the ownership tracking rules will result in a compile time error.

**Example**

: In the following example, the variable `x` is declared as initialized with the value `"hello"`. The value inside `x`
is then moved into the variable `y` on declaration. The variable `x` is now non-initialized, and cannot be used until it
is re-assigned a value (would need to be declared as `mut`).

:
```
let x = "hello"
let y = x
```

## Partial Moves

<secondary-label ref="doc-sect-complete"/>
<secondary-label ref="feature-frozen"/>

Partially-initialized variables are created when, as described above, an attribute is moved off an object. This leaves
the variable in a restricted state; it cannot be moved of borrowed from, until all partial moved have been resolved.

A borrow can never be a partial move. This is because it is not possible to borrow a partially-initialized object, and
is not possible to move an attribute off an object that is borrowed from. Together, these rules ensure that borrows are
always completely valid, mitigating any unexpected behaviour.

**Example**

: In the following example, a partial move is created, when moving the `x` attribute off of the `point`.

:
```
let p = Point(x=0, y=0, z=0)
let x = p.x
```

## Moving vs Copying

<secondary-label ref="doc-sect-complete"/>
<secondary-label ref="feature-not-impl-yet"/>

By default, all values in S++ are moved when assigned to a variable, passed as a function argument, or returned from a
function. This means that the value is moved from the source to the destination, and the source is left in a
non-initialized state.

To copy a value, the `Copy` type must be superimposed on the type. This allows the value to be copied in all instances
where a move would have otherwise happened. This is superimposed on the numeric and boolean classes. As the `Copy` type
is stateless, instead of using `sup Copy on T { }`, it is fine to use `@inherit[Copy]()`.

The default `Copy` functionality is to recursively copy all attributes of the object. Therefore, it requires all the
attributes' types to also superimpose the `Copy` type. As this is a compiler known class, special checks can be done
with it.

For a customized copying behaviour, the `Clone` type can be superimposed on the type. This allows the user to define
their own _cloning_ behaviour, by overriding the `Clone::clone` method. The `Clone` and `Copy` types are completely
independent, and don't require each other by default.

**Example**

: In the following example, the type `Point` is defined with the `Copy` type.

:
```
@inherit[Copy]()
cls Point {
    x: U32
    y: U32
    z: U32
}
```

## Rules

<secondary-label ref="doc-sect-complete"/>
<secondary-label ref="doc-sect-subj-update"/>

1. A value is moved out of a variable when it's type doesn't superimpose `Copy`, and is used as an assignment value,
   move-convention function argument, object initialization argument, move-convention yield, or return value.
2. For an attribute to be movable off an object, the attribute must be fully-initialized. This means that the owner
   object must be either fully-initialized too, or partially-initialized with that attribute present.
3. Moving an attribute off an object will leave the object in a partially-initialized state, with that attribute
   removed.
4. An attribute can be assigned to a fully-initialized or partially-initialized object, but not a non-initialized
   object.
5. A partially-initialized object cannot be moved or borrowed from, until all attributes have been resolved, thus making
   it a fully-initialized object.
6. An attribute can never be moved off an object that is a (mutable) borrow.

## Design Decisions

<secondary-label ref="doc-sect-complete"/>

1. The default semantic is to move values, as this is the most efficient way to handle memory. This feature was inspired
   by Rust.
2. Copying a type is enabled by superimposing the `Copy` type on the type. This was also inspired by Rust, as are the
   entire operations suit - superimpose special compiler-known classes over a type to add or override functionality.
3. Partial moves are allowed, as they are a useful feature in a method that consumes the `self` object, or during
   destructure operations.
4. Partial moves cannot be taken from borrowed object, and borrows cannot be taken from partially-initialized objects.
