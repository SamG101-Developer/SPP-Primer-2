# 8.3. Function Overloading

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Function Overloading

S++ supports function overloading. There are a set of conditions for each overload, to ensure that there aren't any
conflicts. The following items **can** be varied to provide valid overloads of a function identifier:
- **Required parameter types**: The types of the required parameters of overloads must be different. This allows the
  argument matching to select an overload based on the type. If an argument's type doesn't match the corresponding
  parameter type (name-compared then position-compared), then the overload is marked as non-matching.
- **Required parameter conventions**: On top of type-comparisons for argument to parameter matching, the convention of
  the symbol (argument) must match the convention of the parameters. This means that a `&Str` doesn't match
  an `&mut Str`, and therefore conventions can be used to make valid overloads.

The following things **cannot** be used to create overloads:
- **Parameter mutability**: The mutability of a parameter, like for variables, is tied to the variable symbol rather
  than the type of the variable. Therefore, it is impossible to distinguish a call to a function with a mutable vs an
  immutable parameter, as the mutability is only considered inside the target function itself.
- **Self parameter convention**: A `self` argument is never passed manually; it is automatically added to overload
  candidates that have a `self` argument, with the argument convention set to match the parameter convention. If there
  are overloads where only the `self` parameter's convention is different, it would be impossible to detect the correct
  overloads, because both would match perfectly.
- **Optional parameters**: Two overloads that have the same required parameters, but where one has additional optional
  parameters, provides two candidates that match exactly, because leaving the optional arguments set to their defaults
  removes the need for those corresponding arguments. This would leave the "required" signature matching the other
  candidate exactly. Choosing a unique candidate would then be impossible.
- **Variadic parameters**: For the same reason as optional parameters, variadic parameters cannot be used to provide
  overloads. This is because the variadic parameter could be left empty, and then there would be a match the overload
  with the same required parameters.
- **Return type**: S++ uses unidirectional type inference, so the overload cannot be chosen by the type of the variable
  that this function call is being assigned too. This means that the return type has no impact on whether the overload
  is valid, but rather a type mismatch error will be thrown if the return type doesn't match the variable being assigned
  to (if this is the case).
- **Parameter names**: Parameter names are slightly different. They cannot be used to create overloads, because if
  arguments are provided as unnamed, then it is impossible to know which overload to use. However, named arguments'
  names can be used to declare function candidates as non-matching, if the candidate doesn't have any parameters with a
  matching name to the named argument's named provided.

## Conflicting Overloads

Conflicting overloads are detected in two places. The first is when functions are defined, and the second is
post-generic substitution, when the function is called. The compiler will throw an error if it detects a conflict (both
are compile time errors).

## Overload Detection

The first thing to do is consider every function with a matching name, belonging to the correct parent scope. For free
functions, this is the namespace that the function is defined in. For a method, it is the scope belonging to the class
who the method belongs to.

Once the owner scope is determined, every occurrence of preprocessor-generated mock function classes are collected. This
is because free functions can be defined with the same name in different files of the same module, and methods can be
created in separate superimposition blocks. Every overload inside each mock function class is then considered - a flat
list of all overloads (as function prototype ast nodes) is created.

## Overload Consideration

Each of the following checks are sequentially applied to each overload. Matching overloads are marked as `valid`, and
non-matching overloads are marked as `errored`, with the corresponding error attached to the overload.

1. **Number of arguments**: If the number of arguments provided to the function call exceeds the number of parameters
   in the function prototype, then the overload is marked as non-matching. The only exception to this is if the last
   parameter is variadic, in which case the overload is still considered matching.
2. **Argument names (1)**: The next check is to ensure that all the named argument's names provided are parameter names
   in the current function prototype being considered. If there are any argument names invalid for the overload, then
   the overload is `errored`.
3. **Argument conversion**: The next step is to convert all the arguments to named arguments according to the function
   overload under consideration. Each unnamed argument is given the first available parameter name. Named argument
   names are already removed from the list of available parameter names, so they are not considered for this step.
4. **Argument names (2)**: If there are any parameters that haven't been given a value (ie there are fewer arguments
   than parameters), then the overload is `errored`. Doing the length check here allows the name of the parameter that
   hasn't been given an argument to be part of the error message.
5. **Infer generic arguments**: Generic arguments are inferred from the parameters. The list of them are attached to
   the generic arguments provided as part of the function call, and the generic arguments of the enclosing class, if the
   function is a method. Conflicting generic arguments at this stage can result in an `errored` overload.
6. **Generic substitution**: If there are either generic types involved, or the function overload is variadic, then a
   new substituted overload must be created. This replaces all the generic parameters with the corresponding arguments,
   particularly in the parameter names and return type. This simplifies the following type comparisons.
7. **Type checking**: Check every function argument matched the corresponding parameter's type and convention. A single
   mismatch of either type or convention means that the overload is `errored`.

If either 0 or more than 1 overload (from generic substitution) is valid, then the call is ambiguous. If there is only
one valid overload, then the call is considered non-ambiguous, and the function call is resolved to that overload.

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
