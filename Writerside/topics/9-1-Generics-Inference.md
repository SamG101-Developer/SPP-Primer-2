# 9.1. Generics &amp; Inference

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Generics

S++ uses generics, like Rust and Java, to provide a way to write code that is type-agnostic. This allows for code to be
written that is more flexible, and can be re-used in more places. Generics in S++ use the `[]` tokens, like Python,
instead of the commonly used `<>` tokens in Rust and Java. This is because the `<` and `>` tokens are used for boolean
comparisons.

Like C++ and Rust, there are 2 types of generics in S++:
- **Type Generics**: These are the most common type of generics, and are used to define a type that can be any type.
- **Value Generics**: These are used to define a compile-time constant that can be any value, like `Array[T, n]`.

Generics are used in the following ways:
- **Function Generics**: Generics can be used in functions to allow for the function parameters to be of any type. This
  increases the flexibility of the function, and allows for the function to be re-used in more places.

- **Class Generics**: Generics can be used in classes to allow for the class attributes to be of any type.

- **Superimposition Generics**: Generics can be used in superimpositions to allow for either the methods to all use the
  same generic type, or for the super-class to be generic too.

- **Type Alias Generics**: As types can be generic, aliasing requires generic support too, to allow the generics to map
  from the new type to the old type.

## Inference

Generics can be inferred from certain places, depending on the context that the generics have been defined in. The
following table shows the places where generics can be inferred:

<table>
<tr>
<th>Context</th>
<th>Can be inferred from</th>
</tr>

<tr>
<td>Function</td>
<td>

- Argument types -> Parameter types
- Other generic constraints
</td>
</tr>

<tr>
<td>Class</td>
<td>

- Object initialization argument types -> Attribute types
- Other generic constraints
</td>
</tr>

<tr>
<td>Superimposition</td>
<td>

- Type being superimposed over
</td>
</tr>

<tr>
<td>Type Alias</td>
<td>

- No inference
</td>
</tr>
</table>

## Examples

### Function Example

**Inferable Function Generics**

:
```
fun func[T](a: T, b: T) -> Void { }
```

:
```
func(1, 2)
func("hello", "world")
```

:
This example shows the generic type `T` being defined for the function `func`. It can be inferred from the parameters,
because both `a` and `b` are type `T`. If neither parameter was type `T`, the generic would have to be explicitly
defined. In the 2 examples, `T` is inferred as `BigNum` and then `Str`.

**Non-Inferable Generics**

:
```
fun func[T](a: Str, b: Str) -> T { }
```

:
```
func[Str]("hello", "world")
```

:
This example shows a non-inferable function generic, and so it must be passed as an explicit generic type argument.

### Class Example

**Inferable Class Generics**

:
```
cls Vector[T] {
    x: T
    y: T
}
```

:
```
let v = Vector(x=1, y=2)
```

:
This example shows an object initialization setting `x` and `y` to `BigNum` types, allowing `T` to be inferred
as `BigNum`.

### Superimposition Example

**Inferable Superimposition Generics**

:
```
sup [T] SomeClass[T] { ... }
```

:
The generic argument `T` is inferred from a type that's being superimposed over. For example, when `SomeClass[Str]` is
generated, `T` is inferred as `Str` for this superimposition. Superimpositions are re-generated / substituted for each
generically-substituted type that they are superimposed over.
