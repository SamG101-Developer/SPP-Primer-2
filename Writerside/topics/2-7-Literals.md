# 2.7. Literals

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Definition

Literals are fixed values that are written directly into the source code. They are used to represent values of different
types, such as numbers, strings, and booleans. Literals are used to represent constant values that cannot be changed. A
literal can be assigned to a mutable variable, whose contents can be changed.

## Numeric Literals

S++ supports 3 different numeric literals: binary, decimal, and hexadecimal. Binary and hexadecimal literals are the
simplest numeric literals. They are composed of a prefix followed by a sequence of digits:

| Literal Type | Prefix | Example  |
|--------------|--------|----------|
| Binary       | `0b`   | `0b1010` |
| Hexadecimal  | `0x`   | `0x2A`   |

Decimal literals can either be integers or floats. Both can be prefixed by a `-` or `+`, dictating their sign. They can
also contain `_` characters as separators, to make the number more readable.

Decimal float literals must always be prefixed by an integer part, which can be `0`. This means that `.123` is not a
valid float literal, but `0.123` is. Both the integer and float part can use `_` separators.

### Numeric Postfix Literals

All numeric literals can also have a numeric postfix type. This allows the type of the number to be explicitly defined.
If there is no postfix, either the `std::BigInt`, or `std::BigDec` is used. These are the default types for integers and
floats, respectively.

The numeric postfix literals are:

| Postfix | Type        | Description              |
|---------|-------------|--------------------------|
| `_u8`   | `std::U8`   | Unsigned 8-bit integer   |
| `_u16`  | `std::U16`  | Unsigned 16-bit integer  |
| `_u32`  | `std::U32`  | Unsigned 32-bit integer  |
| `_u64`  | `std::U64`  | Unsigned 64-bit integer  |
| `_u128` | `std::U128` | Unsigned 128-bit integer |
| `_u256` | `std::U256` | Unsigned 256-bit integer |
| `_i8`   | `std::I8`   | Signed 8-bit integer     |
| `_i16`  | `std::I16`  | Signed 16-bit integer    |
| `_i32`  | `std::I32`  | Signed 32-bit integer    |
| `_i64`  | `std::I64`  | Signed 64-bit integer    |
| `_i128` | `std::I128` | Signed 128-bit integer   |
| `_i256` | `std::I256` | Signed 256-bit integer   |
| `_f8`   | `std::F8`   | Signed 8-bit float       |
| `_f16`  | `std::F16`  | Signed 16-bit float      |
| `_f32`  | `std::F32`  | Signed 32-bit float      |
| `_f64`  | `std::F64`  | Signed 64-bit float      |
| `_f128` | `std::F128` | Signed 128-bit float     |
| `_f256` | `std::F256` | Signed 256-bit float     |

## String Literals

The string literal syntax is the same as almost every language: a sequence of characters enclosed in double quotes. This
will create an `std::Str` object, which is the only string type in S++. For more information on strings, see
the [STL String]() section.

Single quotes cannot be used to create a string, as this would provide 2 ways to do the same thing, and create an
unnecessary styling difference.

There are no character literals in S++. Either a single-character string literal can be used, or the `std::U8` type can
be used to store a single character. This design decision was made, as C++ code like `'a' + 'b'` would be expected to
return `"ab"`, but actually returns 195, because the characters are implicitly cast to integers.

## Regex Literals

Regex literals are identical to string literals, but the first `"` character is prefixed by an `r`. The type of the
literal is the `std::Rgx` type.

## Boolean Literals

Boolean literals are the simplest literals in S++. They can only be `true` or `false`, and are used to represent the
boolean values `true` and `false`. There are keywords for these values, and as with all keywords, they are reserved
identifiers, and cannot be redefined. The type of booleans is `std::Bool`

## Tuple Literals

Tuple literals are a sequence of values enclosed in parentheses. They can contain any number of values, and can contain
values of different types. Tuple literals are used to create `std::Tup` objects, which are used to store multiple values
in a single object.

There are 3 "types" of tuples (all `std::Tup`), which are used to store different numbers of values:

| Tuple Length | Example Tuple Literal |
|--------------|-----------------------|
| 0            | `()`                  |
| 1            | `(1, )`               |
| 2            | `(1, 2)`              |

Note the extra trailing comma in the 1-tuple literal. This is required to distinguish between a tuple and a single value
in parentheses. For more information on tuples, see the [STL Tuple]() section.
