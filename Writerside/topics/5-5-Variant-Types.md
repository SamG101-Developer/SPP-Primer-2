# 5.5. Variant Types

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

<secondary-label ref="doc-subj-update"/>

## Variant types

S++ variants, are a runtime-tagged type with no syntactic wrapping. This makes them cleaner that Rust's ADTs. They map
to the `std::Var[..Variants]` type, with the generic arguments making up the types inside the variant. Variants are
defined as follows:

```
use MyType = Str | U32 | Bool
```

This creates the object `MyType` with the type `std::Var[Str, U32, Bool]`.

## Special Behaviour

### Construction

> **Todo**: There is currently no way to use a `std::Var` constructor, because the internal value is one of the
> generics. It might be a future feature to allow a variable to be one generic type from a variadic generic argument. This
> supersedes the need for variant types however, so currently this is unknown. At the moment, define the variable as a
> variant type, and separately assign the internal value of a composite type.
> ```
> let a: std::Var[Str, U32, Bool]
> a = "hello"
> ```
> {style="warning"}

Aliased variant construction:

:
```
use MyType = Str | U32 | Bool
let b = MyType("hello")
```

### Assignment

Variants have special behaviour when it comes to assignment. When assigning a value to a variant, the value must either
be a variant type of the exact same generics, or a value of one of the types inside the variant. For example, the
following is valid:

```
use MyType = Str | U32 | Bool

fun function(mut a: MyType, b: MyType) -> Void {
    a = "hello"
    a = b
}
```

### Pattern Matching

See the section on [flow typing](5-9-Flow-Typing.md) for more information on pattern matching.

## Design Decisions

1. There are **no special keywords for variants**. This enables them to be first-class and integrate with the entire
   type system as any other type. They still have special behaviour regarding assignment and pattern matching.
