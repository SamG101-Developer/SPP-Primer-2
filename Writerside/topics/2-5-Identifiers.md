# 2.5. Identifiers

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

## Identifier Definition

An identifier is a sequence of characters that is used to name variables, methods, classes, modules, and other entities.
In S++, there are two types of identifiers: type identifiers and non-type identifiers. They have different syntactic
rules, allowing for clear identification of each, and simpler parsing.

These 2 different types of identifiers have different lexeme rules, meaning that they are differentiated at the parser
(the syntactic level)

## Naming Rules

| Type     | Rule                | Example                        |
|----------|---------------------|--------------------------------|
| Non-Type | `[a-z][_a-z0-9]*`   | `foo`, `bar`, `baz`, `foo_bar` |
| Type     | `[A-Z][a-zA-Z0-9]*` | `Foo`, `Bar`, `Baz`, `FooBar`  |

Identifiers are case-sensitive, meaning that `Foobar` and `FooBar` are different types. Variable identifiers don't
suffer from any case-sensitive ambiguity, as they cannot contain capital letters.

S++ distinguishes between variable and type identifiers at the parser level, by requiring type identifiers to start with
an uppercase character, such as `Str` and `U32`. Following the first (uppercase) character, they can contain any
alphanumeric character. This forces `PascalCase` to be used.

Non-type identifiers (variables, methods etc) must start with a lowercase character, and can only contain lowercase
characters, numbers and underscores, forcing `snake_case` to be used.

Forcing all types to use `PascalCase`, and other identifiers to use `snake_case`, code across multiple libraries will
always follow the same identifier conventions, meaning code will always be consistent and remain readable. This enforces
a "convention" at the syntax level.

As seen in the regex, only ASCII alphanumeric characters are allowed in identifiers. This design decision was made to
force code to be easier to read and understand, as non-ASCII characters can be difficult to distinguish from each other.

There are no length limitations on identifiers, but it is recommended to keep them short and descriptive (see the style
guide).

## Reserved Identifiers

All keywords are reserved identifiers, and cannot be used as variable or type names. This is to prevent ambiguity in the
code, and not make identifiers context-dependent. The list of keywords can be found in the [Keywords](2-6-Keywords.md)
section.

All future keywords will also be become reserved identifiers, creating breaking changes in the language. This is
discussed in the [Versioning]() section.

## Special Identifiers

There are no special identifiers in S++, like Python's magic methods etc. All identifiers are treated equally, and are
parsed the same way. "Special" operations are generally done by superimposing a compiler-known class onto the type and
overriding specific functionality.

Beneath is a translation table of key Python magic methods to the S++ equivalent, to demonstrate the lack of special
identifiers.

| Python Magic Method                        | S++ Equivalent                                                                                 |
|--------------------------------------------|------------------------------------------------------------------------------------------------|
| `__new__`/`__init__`                       | [Uniform Object Initialisation]()                                                              |
| `__del__`                                  | Superimpose `std::Del`, override `del(self) -> std::Void`                                      |
| `__repr__`/`__str__`/`__format__`          | Superimpose `std::From[T]` on the `std::Str` type (see [type conversion]())                    |
| `__bytes__`                                | Superimpose `std::From[T]` on the `std::Bytes` type                                            |
| `__bool__`                                 | Superimpose `std::From[T]` on the `std::Bool` type                                             |
| `__lt__`                                   | Superimpose `std::ops::Lt`, override `lt(self, other: T) -> std::Bool`                         |
| `__le__`                                   | Superimpose `std::ops::Le`, override `le(self, other: T) -> std::Bool`                         |
| `__gt__`                                   | Superimpose `std::ops::Gt`, override `gt(self, other: T) -> std::Bool`                         |
| `__ge__`                                   | Superimpose `std::ops::Ge`, override `ge(self, other: T) -> std::Bool`                         |
| `__eq__`                                   | Superimpose `std::ops::Eq`, override `eq(self, other: T) -> std::Bool`                         |
| `__ne__`                                   | Superimpose `std::ops::Ne`, override `ne(self, other: T) -> std::Bool`                         |
| `__hash__`                                 | Superimpose `std::Hash`, override `hash(self) -> std::U64` (see [object hashing]())            |
| `__getattr__`/`__getattribute__`/`__get__` | Superimpose `std::Rx`, override `attr_get(self, name: std::Str) -> std::Any`                   |
| `__setattr__`/`__set__`                    | Superimpose `std::Rx`, override `attr_set(self, name: std::Str, value: std::Any) -> std::Void` |
| `__delattr__`/`__delete__`                 | Superimpose `std::Rx`, override `attr_del(self, name: std::Str) -> std::Void`                  |
| `__dir__`                                  | Has no use in S++                                                                              |

## Identifier Scope

S++ uses strict block scoping to contain identifiers and prevent the leaking of variables. This means that variables
declared in a block are only accessible within that block, and not outside of it. This is to provide a clear and
predictable scope for variables, and to prevent accidental variable shadowing.

An identifier from an outer block can be accessed from an inner block, but an identifier from an inner block cannot be
accessed from an outer block. See the section on [shadowing]() for more information on defining variables with the same
name in an inner block.

The lifetime of a variable is the lifetime of the block it was defined in. The lifetime of a variable can be extended by
returning it out of a function, either as the variable itself, or by first moving it into the attribute of an object,
and returning that object. Either way, the variable must be moved out of the function to extend its lifetime. This works
because the variable is now part of the outer frame, and as such, its lifetime is extended to the lifetime of the outer
frame.

