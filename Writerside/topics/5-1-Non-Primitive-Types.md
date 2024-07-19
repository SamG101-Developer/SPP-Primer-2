# 5.1. (Non) Primitive Types

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

## Primitive Types

Typically, a language has a selection of primitive types. In C++, this
includes `int`, `char`, `float`, `double`, `void`, `[]`; and in Rust this includes `i32`, `u32`, `f32` etc. In S++,
there are no primitive types - the Java "everything is an object" philosophy is followed. This means that all types are
first-class, providing a uniform and orthogonal type system, eliminating the need for special cases and rules for
primitive types.

There are some compiler-known types, which are used in place of primitive types. These types map to LLVM types, allowing
for efficient code generation.

## Numeric types

S++ has 3 sets of numeric types - unsigned integers, signed integers, and floating point numbers. Each of these types
support 8 to 256 bits. These types cannot be instantiated, but can be used
with [numeric postfix literals](2-7-Literals.md#numeric-postfix-literals) to create instances of these types. The types
are:

<tabs>
<tab id="U" title="Unsigned Integers">

| Type   | Size (bits) | Min | Max     | Precision | Description              |
|--------|-------------|-----|---------|-----------|--------------------------|
| `U8`   | 8           | 0   | 2^8 - 1 | 8 bits    | Unsigned 8-bit integer   |
| `U16`  | 16          | 0   | 2^16- 1 | 16 bits   | Unsigned 16-bit integer  |
| `U32`  | 32          | 0   | 2^32- 1 | 32 bits   | Unsigned 32-bit integer  |
| `U64`  | 64          | 0   | 2^64- 1 | 64 bits   | Unsigned 64-bit integer  |
| `U128` | 128         | 0   | 2^128-1 | 128 bits  | Unsigned 128-bit integer |
| `U256` | 256         | 0   | 2^256-1 | 256 bits  | Unsigned 256-bit integer |

</tab>
<tab id="I" title="Signed Integers">

| Type   | Size (bits) | Min    | Max      | Precision | Description            |
|--------|-------------|--------|----------|-----------|------------------------|
| `I8`   | 8           | -2^7   | 2^7 - 1  | 7 bits    | Signed 8-bit integer   |
| `I16`  | 16          | -2^15  | 2^15 - 1 | 15 bits   | Signed 16-bit integer  |
| `I32`  | 32          | -2^31  | 2^31 - 1 | 31 bits   | Signed 32-bit integer  |
| `I64`  | 64          | -2^63  | 2^63 - 1 | 63 bits   | Signed 64-bit integer  |
| `I128` | 128         | -2^127 | 2^127-1  | 127 bits  | Signed 128-bit integer |
| `I256` | 256         | -2^255 | 2^255-1  | 255 bits  | Signed 256-bit integer |

</tab>
<tab id="F" title="Floating Points">

| Type   | Size (bits) | Min    | Max      | Precision | Description          |
|--------|-------------|--------|----------|-----------|----------------------|
| `F8`   | 8           | -2^7   | 2^7 - 1  | 7 bits    | Signed 8-bit float   |
| `F16`  | 16          | -2^15  | 2^15 - 1 | 15 bits   | Signed 16-bit float  |
| `F32`  | 32          | -2^31  | 2^31 - 1 | 31 bits   | Signed 32-bit float  |
| `F64`  | 64          | -2^63  | 2^63 - 1 | 63 bits   | Signed 64-bit float  |
| `F128` | 128         | -2^127 | 2^127-1  | 127 bits  | Signed 128-bit float |
| `F256` | 256         | -2^255 | 2^255-1  | 255 bits  | Signed 256-bit float |

</tab>
</tabs>

## Boolean type

The `std::Bool` type is used to represent the type of either `true` or `false`. Both `true` and `false` are
program-lifetime constants. Semantically, they work as if `Copy` has been superimposed over them. The `std::Bool` S++
type maps to the LLVM `bool` type.

## Void type

S++ requires all functions to provide their return type, even if no object is being returned. Therefore, the `Void` type
is required, to tell the compiler to expect no objects being returned. The `Void` type cannot be used for attributes. If
a function parameter is generic, and the generic argument provided is `Void`, then the parameter is removed from the
signature. This is seen in the generator types, where no value is sent with `.next()` for `Send=Void` generators, and
otherwise `.next(1)` might be used to send a `std::BigInt` into the generator. The `Void` type is mapped to the LLVM
`void` type.
