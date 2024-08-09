# 5.2. Arrays

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

<secondary-label ref="doc-subj-update"/>

## Arrays

S++ has a `Arr[T]` type, which represents an array as a first-class object. It is a fixed-size collection of elements of
the same type. Unlike C++ and Rust's primitive array types, S++'s arrays don't have any different treatment or syntax.
This maintains the strict orthogonality and readability of the language.

The `Arr[T]` class uses both bounds checking and optional return types to enforce safety. This it impossible for arrays
to create low-level errors that can be found in other low level languages. The `UnsafeArr[T]` class is composed and
wrapped in bound checks for the `Arr[T]` class's methods.

## Array Safety

All array access operations are bounds checked, to ensure that the memory being accessed in within the bounds of the
array. This is to prevent buffer overflows and other memory-related bugs. The bounds check is done with a branch
prediction hint that the index is within bounds.

All accessor return `Opt[T]` types, in-case the access it either out of bounds, or there isn't an element in the array
at the index. There is no special `[]` syntax for indexing, as these tokens are reserved for generics. There is a
variadic static `new` method for the `Arr[T]` class, which allows for the array to be initialized with multiple
elements. The array is sized to the number of elements passed to the `new` method.

Accessors are coroutines, allowing borrows to be yielded. This is because only owned objects can be
stored ([2nd class borrow system](11-3-Second-Class-Borrows.md)), and not borrows.
