# 5.13. Type Casting / Conversion

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

S++ employs a strong typing system. This means that all implicit casting is impossible. This extends to class
inheritance upcasting; in C++, `Derived*` can be cast to `Base*` without any explicit casting. In S++, this is not
allowed.

## Inheritance Casting

These 2 functions are compiler known and have no visible S++ implementation.

```
fun upcast[Base, Derived: Base](obj: Derived) -> Base { }

fun downcast[Base, Derived: Base](obj: Base) -> Opt[Derived] { }
```

## Regular Casting

There is no specific syntax for casting between types. To keep all casts uniform, the `From` type can be superimposed
over classes, to allow a type to be cast into that type:

```
sup From[Origin] {
    fun from(that: Self::Origin) -> Self { }
}
```

This can be used as follows:

```
cls Foo { }
cls Bar { }
cls Baz { }

sup From[Bar] on Foo {
    fun from(that: Self::Origin) -> Self { ... }
}

sup From[Baz] on Foo {
    fun from(that: Self::Origin) -> Self { ... }
}

fun main() -> Void {
    let x = Bar()
    let y = Foo::from(x)
}
```

## Design Decisions

1. There is **no specific type casting syntax**. This is because static methods can do the same thing, so there would be
   2 ways to do the same thing. Furthermore, the `From` type can be used as a constraint for a generic type, so it is
   more versatile.
