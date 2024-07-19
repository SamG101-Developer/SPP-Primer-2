# 5.2. Arrays

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

<secondary-label ref="doc-subj-update"/>

## Arrays

S++ has a built-in array type, which is a fixed-size collection of elements of the same type. It doesn't have any
specific syntax or special treatment, and is just a class like any other. The corresponding first-class type
is `std::Arr[T]`. It is a compiler-known class, and maps to the LLVM `array` type.

All array operations are bounds checked, with a branch prediction hint that the index is within bounds. This is to
prevent buffer overflows and other memory-related bugs. There is no `[]` syntax for arrays, as these tokens are reserved
for generics.

The `std::Arr[T]` type has a variadic static `new` method, which allows for the array to be initialized with multiple
elements. There are accessor methods getting, setting and deleting elements. Accessor operations return
the `std::Opt[T]` type, in-case the bounds check fails, or an element is not present at the index.

Accessors are defined as coroutines, so that borrows can be taken from elements within the array. Arrays themselves can
only store owned objects, and **not** borrows.
