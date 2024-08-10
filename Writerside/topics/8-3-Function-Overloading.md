# 8.3. Function Overloading

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Function Overloading

S++ supports function overloading. There are a set of conditions for each overload, to ensure that there aren't any
conflicts. The following items can be varied in order to provide an overload:
- Function required-parameter types.
- Function required-parameter conventions.

The following things cannot be used to create overloads:
- Function self-parameter convention.
- Function parameter mutability.
- Adding optional/variadic parameters to matching required parameters.
- Function return type.
- Function parameter names.

![OverloadFlowchart.png](OverloadFlowchart.png) {width=300}

## Valid Function Overload Examples

**Parameter type overload:**

:
```
fun function(a: Str) -> Str { }
fun function(a: U32) -> Str { }
```

:
Type based overloading works because the type of every expression can be inferred at compile time, providing a way to
select different overloads.

**Parameter convention overload:**

:
```
fun function(a: Str) -> Str { }
fun function(a: &Str) -> Str { }
```

:
Convention based overloading works because borrows must be taken manually with the `&` or `&mut` tokens, providing a
clear distinction to match against.

## Invalid Function Overload Examples

**Self parameter**

:
```
fun method(&self, a: Str) -> Str { }
fun method(&mut self, a: Str) -> Str { }
```

:
Overloading based on the `self` parameter is not supported, because the `self` argument is never provided with a
convention, making it impossible to select the correct overload.

:
Whilst a `&mut self` method couldn't be applied to an immutable object, a mutable object could fit in `&self`
and `&mut self`. Therefore, `self` based overloading isn't supported.

**Parameter mutability**

:
```
fun function(a: Str) -> Str { }
fun function(mut a: Str) -> Str { }
```

:
Mutability is tied to the value, and not the type of value. This means that the `mut` has no bearing on the argument
being passed into the parameter, only on the parameter itself. As such, the `mut` doesn't affect the signature, and
therefore cannot be used to provide overloads.

**Optional/variadic parameters**

:
```
fun function(a: Str) -> Str { }
fun function(a: Str, b: Str = "default") -> Str { }
fun functoin(a: Str, ..b: Str) -> Str { }
```

:
As optional and variadic parameters are not required, they cannot be used to provide overloads. The compiler will always
check required-parameters only, for overload conflict analysis.

**Function return type**

:
```
fun function(a: Str) -> Str { }
fun function(a: Str) -> U32 { }}
```

: S++ only supports 1-way type inference, meaning that the return type cannot be selected based on the `let` type -
the `let` type is always inferred from the function call. Therefore, the return type cannot be used to provide
overloads.

**Parameter names**

:
```
fun function(a: Str) -> Str { }
fun function(b: Str) -> Str { }
```

:
The function signature ignored parameter names, and only analyses types and conventions. Therefor overloading based on
parameter names is not supported.

## Valid Overloads, Ambiguous Calls

Even though there is a strict criteria for providing overloads, at function calls, there can still be ambiguity, if
generics are involved. Generally these can be resolved, by choosing the "most-matching" overload, but if there are two
overloads of equal match, the call will be ambiguous.

Function **match-tuples** are created, which measure how "close" a match is between a function call, and a function
definition. Each tuple is summed, and the tuple with the smallest sum is the most matching.

```
fun function(a: Str) -> Str { }
fun function[T](a: T) -> Str { }

fun main() -> Void {
    function("string")
}
```
The function signatures' match-tuples are `(0) => 0` and `(1) => 1`. The first overload is therefore more precise (a
closer match), and is selected as the overload to call. Any other type, such as `function(123)`, would call the generic
overload; `123` mis-matches with `Str`, meaning that the `Str` overload wouldn't even be considered and therefore
wouldn't require match-tuple comparisons.

However, if the overloads are equally matched, the call will be ambiguous:

```
fun function[T](a: Str, b: T) -> Str { }
fun function[T](a: T, b: Str) -> Str { }

fun main() -> Void {
    function("hello", "world")
}
```

The function signatures' match-tuples are `(0, 1) => 1` and `(1, 0) => 1`, both creating equally matching overloads. At
this point, a compile-time error is thrown. Compile time errors are not thrown at the definition of these functions,
because there are a lot of scenarios where non-ambiguous calls can be made, but compile-time analysis can detect the
cases where ambiguity occurs.
