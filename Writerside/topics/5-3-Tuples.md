# 5.3. Tuples

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

## Tuples

Tuples are a fixed-size collection of elements of different types. They are first-class objects, and are used to store
multiple values in a single object. The corresponding first-class type is `std::Tup[..Ts]`. It is a compiler-known
class, and maps to the LLVM `struct` type.

Tuple construction syntax mirrors an object instantiation, without the class name or attribute names. For example,
instead of `Point(x=1, y=2, z=3)`, the syntax is `(1, 2, 3)`. See the section
on [tuple literals](2-7-Literals.md#tuple-literals) for more information on tuple literals.

## Binary Folding

Tuples can be folded into a single value, as long as each value can be combined with the next value, with the operation
specified. Tuples can be binary folded from the left, or the right, depending on the syntax used:

Left Binary Fold

:
```
fun main() -> Void {
    let tuple = (1, 2, 3, 4, 5)
    let sum = .. + tuple
    std::print(sum)  # 15
}
```

Right Binary Fold

:
```
fun main() -> Void {
    let tuple = (1, 2, 3, 4, 5)
    let sum = tuple + ..
    std::print(sum)  # 15
}
```

## Function folding

Tuples can also be folded into a single value by continuously applying each element to a function. A tuple of all the
elements returned from the function is returned. Multiple tuples can be folded into a function, as long as they contain
the same number of elements.

In the case where 2 tuples are passed as arguments, but only 1 should be folded, type checking ensures that the correct
tuple is folded (examples will show this). Ambiguities can arise if two signatures exist where folding either tuple is
valid. In this case, the standard ambiguous function call error is raised.

Valid example

: The example beneath will fold the tuple `y` into the argument `b`. Because `x` is being passed as a borrow, it will be
valid for each function call per tuple element. If `x` was being moved into the function, the function could only be
called once, as the value stored by `x` would no longer exist. If the type of `x` superimposed `Copy`, then it would
also work as passing by move.

:
```
fun function(a: &Str, b: Str) -> Void { }
fun main() {
    let x = "hello"
    let y = ("a", "b", "c")
    function(&x, y)..
}
```

Invalid example

: The following example shows a case where an ambiguity would arise. This is because there are 2 signatures where each
tuple could be folded into the function, and the other tuple can be passed by value.

:
```
fun function(a: U32, b: (U32, U32)) -> Void { }
fun function(a: (U32, U32), b: U32) -> Void { }
fun main() -> Void {
    let x = (1, 2)
    let y = (3, 4)
    function(x, y)..
}
```

## Unpacking

Tuple unpacking is the same as the `std::apply` function in C++, but is a language feature. It allows for additional
arguments to be supplied to the function too, meaning that partial functions with `std::apply` isn't required. Any type
can be inside the tuple, as this operation simply destructures the tuple into all of its individual components.

```
fun function_1(x: Bool, a: U32, b: U32, c: U32, y: Bool) -> U32 {
    return a + b + c
}

fun main() {
    let tuple = (1, 2, 3)
    let result = function_1(true, ..tuple, false)
}
```

## Indexing

Tuples values, and types, can be indexed using the `.` or `::` operator. This is the same as an attribute access, and
therefore partial-move memory rules apply to the tuple as-well.

Tuple value indexing

:
```
let tuple = (1, 2, 3)
let a = tuple.0
let b = tuple.1
let c = tuple.2
```

Tuple type indexing

:
```
use TupleType = (Str, Str, Str)
use TT0 = TupleType::0
use TT1 = TupleType::1
use TT2 = TupleType::2
```

## Destructuring

Tuple destructuring is covered in the [variable declaration](4-1-Variables.md#tuple-destructure) section.
