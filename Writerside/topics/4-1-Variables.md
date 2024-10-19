# 4.1. Variables

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Defining a Variable

The `let` statement is used to define a variable, introducing a new symbol into the current scope's symbol table.
The `let` statement can introduce an initialized or uninitialized variable. If the variable is non-initialized, then is
must be given a type annotation, as there is no value to infer from. As an initialized variable's value can always be
fully type-inferred, provided a type annotation is not syntactically allowed, as it introduces redundancy and extra
noise.

## Non-initialized Variables

A non-initialized variable can be defined, but cannot be used until it has been assigned a value.
The [memory model](11-1-Memory-Model.md) uses [ownership tracking](11-2-Ownership-Tracking.md) to monitor the owner of
the variable, and will not allow the variable to be used until it is initialized.

If a variable is non-initialized, and only initialized in some branches of a conditional block, then a "potentially
uninitialized" error will be thrown - a variable must either be initialized or non-initialized at the end of every
branch of a conditional block.

The following syntax is used to define a non-initialized variable:

```
let variable_name: Type
```

## Initialized Variables

An initialized variable contains a left-hand-side part, and a value part. The left-hand-side can be one of:

- Single variable name
- Tuple destructure
- Object destructure
- Combination of the above with nesting

A type annotation cannot be provided for initialized variables, because the type of the value would have to be inferred
to check it matches the provided type annotation anyway, so it is redundant. Therefore, type annotations are not allowed
for initialized variables, and full type inference is used.

### Single Variables

Variables can be defined with a single identifier - this is the most common way to define a variable. The syntax is:

```
let variable_name = value
```

### Tuple Destructure

Variables can be defined by destructuring a tuple. The syntax is:

```
let (variable_name_0, variable_name_1) = tuple_value_0
```

Underscores can be used to skip values in the tuple. The syntax is:

```
let (variable_name_0, _, _, variable_name_3) = tuple_value_1
```

Ellipsis can be used to skip 1 group of values in the tuple. The syntax is:

```
let (variable_name_0, .., variable_name_1) = tuple_value_2
```

Ellipsis can be used to bind multiple values in the tuple to a single variable. The syntax is:

```
let (variable_name_0, ..variable_name_1) = tuple_value_3
```

### Object destructure

Variables can be defined by destructuring an object. The syntax is:

```
let Point(x, y, z) = point_value
```

Ellipsis can be used to skip the rest of the object's attributes. The syntax is:

```
let Point(x, y, ..) = point_value
```

### Nested Destructure

Variables can be defined by destructuring a nested structure. The syntax is:

```
let Vec(point=Point(x, ..), ..) = vec_value
```

## Mutability

Variables are immutable by default. This encourages safer programming, as it prevents unexpected and accidental changes
to variables. To make a variable mutable, the `mut` keyword must be used with the `let` keyword. To make a parameter
mutable, prefix its identifier with the `mut` keyword. Tuple and struct parts must be individually marked as mutable.

In S++, mutability isn't tied to a type, but to the variable itself. This means that the mutability is stored as symbol
metadata, and not in the type system. Examples of variable mutability are shown below:

```
let mut x = 1                          # x is mutable
let (mut x, y) = (1, 2)                # x is mutable, y is immutable
let Point(mut x, y) = Point(x=1, y=2)  # x is mutable, y is immutable
```

```
fun function(mut x: BigNum, z: BigNum) -> Str {
    # x is mutable
    # z is immutable
}
```

Taken from Rust, interior mutability is utilized to simplify the mutability model of objects and attributes. The
mutability of an attribute is dictated by the outermost object containing this attribute. This means that for an
attribute's attribute to be changed, the outermost object must be marked `mut`.

> The mutability of a value and the mutability of a borrow are 2 different things. See the section
> on [borrowing](11-3-Second-Class-Borrows.md) for more information.
> {style="warning"}

## Variable Scope

S++ uses strict block scoping to contain variables and prevent them leaking out of scopes. This means that variables
declared in a block are only accessible within that block, and not outside of it. This is to provide a clear and
predictable scope for variables, and to prevent accidental variable shadowing.

A variable from an outer block can be accessed from an inner block, but a variable from an inner block cannot be
accessed from an outer block. See the section on [shadowing](#variable-shadowing) for more information on defining
variables with the same name in an inner block.

The lifetime of a value is the lifetime of the block it was defined in. The lifetime of a value can be extended by
returning it out of a function, either as the variable itself, or by first moving it into the attribute of an object,
and returning that object. Either way, the variable must be moved out of the function to extend its lifetime. This works
because the variable is now part of the outer frame, and as such, its lifetime is extended to the lifetime of the outer
frame.

## Variable Re-declaration

Variables can be re-declared in the same scope, including changing the type and mutability of the variable. This is the
same as [variable shadowing](#variable-shadowing), but the old variable is discarded as the new variable is residing in
the same scope, overriding the symbol.

```
let x = 1            # x is a std::BigInt
let mut x = "hello"  # x is mutable and a std::Str

```

## Variable Assignment

Variables can be assigned to using the `=` operator. S++ uses a very strong type-system, meaning that not even upcasting
is done automatically. This means that the type of the right-hand-side must match the type of the left-hand-side
explicitly. This is to prevent accidental type conversions, which can be dangerous. Assignment can only be performed on
mutable variables, or uninitialized variables.

## Variable Shadowing

A variable that has been declared in an outer scope can be shadowed by a variable with the same name in an inner
scope. If a variable is shadowed, then inside the inner scope, the new variable is used, and once the scope has
ended, the original variable is used again. This is useful for re-using variable names, and for readability.

### Shadowing example #1:

```
fun main() -> Void {
    let x = 1
    {
        let x = 2
        std.print(x)  # prints 2
    }
    std.print(x)  # prints 1
}
```

### Shadowing example #2:

```
fun main() -> Void {
    let mut x = 1
    {
        x = 2
        std.print(x)  # prints 2
    }
    std.print(x)  # prints 2
}
```

## Global Constants

See the section on [global constants](4-3-Global-Constants.md) for more.