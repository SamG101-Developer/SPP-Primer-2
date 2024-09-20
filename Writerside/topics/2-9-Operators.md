# 2.9. Operators

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Binary Operators

### Precedence

| Operator | Name                       | Class                    | Precedence |
|----------|----------------------------|--------------------------|------------|
| `=`      | Assignment                 | `N/A`                    | 0          |
| `??=`    | Null coalescing assignment | `N/A`                    | 0          |
| `+=`     | Addition assignment        | `std::ops::AddAssign`    | 0          |
| `-=`     | Subtraction assignment     | `std::ops::SubAssign`    | 0          |
| `*=`     | Multiplication assignment  | `std::ops::MulAssign`    | 0          |
| `/=`     | Division assignment        | `std::ops::DivAssign`    | 0          |
| `%=`     | Remainder assignment       | `std::ops::RemAssign`    | 0          |
| `%%=`    | Modulo assignment          | `std::ops::ModAssign`    | 0          |
| `**=`    | Exponentiation assignment  | `std::ops::PowAssign`    | 0          |
| `??`     | Null coalescing            | `std::ops::NullCoalesce` | 1          |
| `\|\`    | Logical OR                 | `std::ops::Or`           | 2          |
| `&&`     | Logical AND                | `std::ops::And`          | 3          |
| `==`     | Equality                   | `std::ops::Eq`           | 4          |
| `!=`     | Inequality                 | `std::ops::Ne`           | 4          |
| `>`      | Greater than               | `std::ops::Gt`           | 4          |
| `<`      | Less than                  | `std::ops::Lt`           | 4          |
| `>=`     | Greater than or equal to   | `std::ops::Ge`           | 4          |
| `<=`     | Less than or equal to      | `std::ops::Le`           | 4          |
| `<=>`    | Comparison                 | `std::ops::Cmp`          | 4          |
| `+`      | Addition                   | `std::ops::Add`          | 6          |
| `-`      | Subtraction                | `std::ops::Sub`          | 6          |
| `*`      | Multiplication             | `std::ops::Mul`          | 7          |
| `/`      | Division                   | `std::ops::Div`          | 7          |
| `%`      | Remainder                  | `std::ops::Rem`          | 7          |
| `%%`     | Modulo                     | `std::ops::Mod`          | 7          |
| `**`     | Exponentiation             | `std::ops::Pow`          | 7          |

#### Non-Token Operators

The following table contains operators that don't have tokens, as this simplifies the language. The classes & methods
are still part of the operators section of the standard library.

| Operator | Name         | Class              |
|----------|--------------|--------------------|
| `N/A`    | Binary OR    | `std::ops::BitIor` |
| `N/A`    | Binary AND   | `std::ops::BitAnd` |
| `N/A`    | Binary XOR   | `std::ops::BitXor` |
| `N/A`    | Binary NOT   | `std::ops::BitNot` |
| `N/A`    | Left shift   | `std::ops::BitShl` |
| `N/A`    | Right shift  | `std::ops::BitShr` |
| `N/A`    | Left rotate  | `std::ops::BitRol` |
| `N/A`    | Right rotate | `std::ops::BitRor` |

### Comparison Operator Chaining

Like in Python, S++ allows for simplified chaining of binary comparison operators. For example, `1 < 2 < 3` is
automatically expanded to `1 < 2 and 2 < 3`. This requires the type to superimpose the `Copy` type, as if the value is a
variable, then it will be consumed as the RHS of the first comparison, but still requires for the second operation.

Despite the operators `<=>` and `is` being used for comparisons, they **cannot** be used for simplified chaining. Only
the operators `<`, `>`, `<=`, `>=`, `==`, and `!=` can be chained.

### Operator Overloading

Like in Rust, operator overloading works by superimposing the operator class over the type, and overriding the method
associated with the operator. Operator classes are generic, allowing for any types to be overloaded with the operation,
but they default to `Self` for simpler default implementation.

```
cls Vec3D {
    x: U32
    y: U32
    z: U32
}

sup std::ops::Add[Rhs=Vec3D] on Vec3D {
    fun add(self, that: Vec3D) -> Vec3D {
        ret Vec3D(
            x=self.x + that.x,
            y=self.y + that.y,
            z=self.z + that.z)
    }
}
```

### Equality

The equality operator has 2 different uses, handled by the compiler. If the left-hand-side is an owned object, then
the `std::ops::Eq::eq` method is used. If the left-hand-side is a borrow, then the right-hand-side must also be a
borrow, and the equality is determined by whether the two borrows borrow the same owned object.
