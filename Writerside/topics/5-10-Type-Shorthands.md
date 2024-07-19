# 5.10. Type Shorthands

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

S++ uses a small selection of shorthand syntax for types, to make the code more readable and concise. These shorthands
are:

| Shorthand           | Type           | Example                     |
|---------------------|----------------|-----------------------------|
| `?`                 | `std::Opt[T]`  | `?Str`                      |
| <code>&#124;</code> | `std::Var[Ts]` | <code>Str &#124; U32</code> |
| `()`                | `std::Tup[Ts]` | `(Str, U32)`                |

