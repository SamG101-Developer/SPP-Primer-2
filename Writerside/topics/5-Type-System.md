# 5. Type System

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

**Contents**

- [](5-1-Non-Primitive-Types.md)
- [](5-2-Arrays.md)
- [](5-3-Tuples.md)
- [](5-4-Function-Types.md)
- Union Types
- Intersection Types
- Residual Types
- Affine Typing
- Flow Typing
- Type Shorthands
- Type Inference
- Type Aliasing
- Type Casting/Conversion

S++ employs a strong and static type system. This requires all types to be known at compile time, and a blanket ban on
any type of automatic type conversion, including upcasting. Every type is a first-class object, including numbers,
booleans, voids, arrays, tuples, unions, functions and classes. This uniformity allows only one set of rules to be
applied to all types, and eliminates the need for special cases and rules for primitive types. All these types can
therefore be passed as arguments, returned as functions, assigned to variables, and so on.

Type inference is used extensively, such that providing a type annotation to an initialized value is syntactically
forbidden. This improves the readability of the code.


The S++ type system is strongly and statically typed. This means that all types are known at compile time, and that
types cannot be implicitly converted. Every type is a first-class object, including arrays, tuples, unions, functions
and classes. This means that they can all be passed as arguments, returned from functions, and assigned to variables,
enforcing the orthogonality of the language.

The type system is so strong, that even implicit upcasting is not allowed; the special `std::upcast` method is required.
The upcast method is constrained to take an argument that superimposes the target type, and the downcast equivalent
function returns an `Opt[T]`, as there is no guarantee which subclass the superclass might be a part of.

There are no "primitive" types with special syntax, like in C++. All types are defined in the standard library, and some
are known to the compiler, such as the integer-based classes, the boolean and void type. Again, these are all
first-class objects, so their semantic behaviour is the same as any object.

Type inference is used extensively, to the point that specifying a type for local variables that are given a value is
not allowed. This is because a value's type can **always** be inferred from the value, and so specifying the type is
redundant. This also improves the readability of the code, as the type is not repeated.

Aliasing is supported in S++, allowing for the same type to be given multiple names. This is useful for when a type is
used in multiple places, but the name of the type is too long to be used everywhere. Furthermore, this allows for
importing types from namespaces into the current namespace.

There is no special syntax for casting, as the same behaviour can be achieved using standard function calls. To ensure
that all casts are normalized, however, the `From` type can be generically superimposed over a type, to allow conversion
from another type into it. This allows `Str.from(123)` to be used to convert an integer into a string, for example.

Generic types are supported and can be used with functions, classes, aliasing and superimposing. Generic parameters can
be required, optional or variadic, and can be constrained to a specific type or set of types. Constraints on `sup` block
generics allow for methods to optionally be added to a class, depending on the generic type.

Residual types are types that superimpose the `std.Residual` class. This class is used to represent a type that has a
fail and success state, and is used for error handling. The `Ret[Pass, Fail]` and `Opt[Type]` types superimpose this
class, and are analogous to `Result<T, E>` and `Option<T>` in Rust.

Union types are mapped to the `std.Var[Ts]` variant class, were the generic parameter `..Ts` is a variadic list of
types. This class is used to represent a type that can be one of multiple types, and is used for branching. It is
commonly seen with the `is` keyword, in pattern matching. The `|` token is used for union type shorthand.

Flow typing is used with union types, to allow for a type to be treated as a possible internal type, within the scope of
the pattern match being considered. This reduces the amount of casting that is required, and improves the readability of
the code.

Arrays are the lowest level collection in S++, and are completely safe to use. See [Arrays](6-1-Arrays.md) for more.

Tuples are first-class objects, like in Rust. See [Tuples](6-4-Tuples.md) for more.

