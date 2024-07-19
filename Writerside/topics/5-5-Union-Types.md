# 5.5. Union Types

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

<secondary-label ref="doc-subj-update"/>

## Union types

S++ unions, also referred to as _variants_, are a runtime-tagged type with no syntactic wrapping. This makes them
considerable cleaner that Rust's ADTs. They map to the `std::Var` type, with the generic arguments making up the types
inside the variant. Variants are defined as follows:

```
use MyType = Str | U32 | Bool
```

This creates the object `MyType` with the type `Var[Str, U32, Bool]`.

### Common Uses

Common uses of the variant type include the `Opt[Type]` type, and the `Res[Pass, Fail]` type.

## Special Behaviour

### Construction

Variants can be constructed by using the `Var` constructor. This constructor takes a single argument, which is a value
that is one of the types in the variant. This will wrap the type inside the variant.

Raw variant construction:

:
```
let a = Var[Str, U32, Bool]("hello")
```

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

See the section on [flow typing]() for more information on pattern matching.

## Design Decisions

1. There are **no special keywords for variants**. This enables them to be first-class and integrate with the entire
   type system as any other type. They still have special behaviour regarding assignment and pattern matching.
