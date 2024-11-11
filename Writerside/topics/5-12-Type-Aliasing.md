# 5.12. Type Aliasing

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Type Aliasing

S++ allows for 2 types of aliasing: "namespace reduction" and "type aliasing". Namespace reduction allows for bringing
in names that are in a namespace, (another module), such as `use std::Str`, which brings in the `Str` type from the
`std` module. Type aliasing allows for more advanced aliasing, involving generic arguments. There is an overlap in the
two methods, which is discussed below.

The syntax mixes Rust's `use` and `type` syntax with C++'s single keyword syntax:

| C++ Syntax                                                                       | Rust Syntax                                | S++ Syntax                                   |
|----------------------------------------------------------------------------------|--------------------------------------------|----------------------------------------------|
| `using String = std::string;`                                                    | `type String = std::Str`                   | `use String = std::Str`                      |
| `template <typename T> using Vec = std::vector<T>;`                              | `type Vec<T> = std::Vec<T>`                | `use Vec[T] = std::Vec[T]`                   |
| `template <typename T> requires std::integral<T> using IntVec = std::vector<T>;` | `type Vec<T: std::Integral> = std::Vec<T>` | `use IntVec[T: std::Integral] = std::Vec[T]` |
| `using namespace std;`                                                           | `use std::*`                               | `use std::*`                                 |
| `using std::string;`                                                             | `use std::Str`                             | `use std::Str`                               |
| `N/A`                                                                            | `use std::{Str as String, BigInt as Int}`  | `use std::{Str as String, BigInt as Int}`    |

## Type Aliasing Statement

Type aliasing allows for types to be aliased wth generic support, and is done with the `use` keyword. The syntax closely
follows standard `let` declarations:

```
cls AAA[A, B, C, D] { }
use BBB[A, B, C] = AAA[A, B, C, std::Bool]
use CCC[A, B] = BBB[A, B, std::BigInt]
use DDD[A] = CCC[A, std::Str]
```

All types in the `[]` tokens are always generic parameters, as the generic arguments will be on the RHS, bound to the
type. This differs from the `sup` block, where the generic parameters have to be explicitly defined in the `[]` before
the type, for specialization.

## Namespace Reduction

Namespace reduction is the simplest form of aliasing, and is used to bring in names from another module. This is done
using the `use` keyword, followed by the module name and the name to bring in.

### Single & Multi Type Reduction

A single type can be imported from another module, The syntax is as follows:

```
use std::Str
use std::{Void, BigInt}
```

### Namespace Reduction Aliases

Types can be aliased during their namespace reduction, by using the `as` keyword. This is useful when there is already a
type of that name in the scope, and the imported type needs to be renamed.

```
use std::Str as String
```

### Layered Reduction

The brace tokens are used for grouping, because _layered_ reduction is supported. This is reflected in the syntax, by
showing the _scope_ of the type in moved into when layered reductions are happening. This is shown below:

```
use std::{
    Str as String,
    BigInt,
    ops::{
        Add,
        Sub,
    }
}
```

### Overlap with Type Aliasing

There is an overlap with the type aliasing syntax:

```
use std::Str as String
use String = std::Str
```

These are identical.
