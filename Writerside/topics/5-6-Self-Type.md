# 5.6. Self Type

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Self Type

The `Self` type is a keyword that refers to the enclosing class that it is used in. It can be used in any `cls` or `sup`
block, and only in `fun` block that reside inside a `sup` block.

## Restrictions

The `Self` type cannot be used as a generic argument in a `sup` identifier, or as a `cls` generic argument default type,
as this would lead to a recursive type.

```
cls A[T] { }

sup A[T=Self] { }
```

This is an error, because the `Self` refers to `A[Self]`, which in turn refers to `A[A[Self]]`, and so on infinitely.
Therefore, a compile time error will be thrown, stating that the type is recursive.

Similarly, the following is also an error, as a recursive type is created:

```
cls A[T=Self] { }
```
