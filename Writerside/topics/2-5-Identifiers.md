# 2.5. Identifiers

<primary-label ref="header-label"/>

<secondary-label ref="complete"/>

## Identifier Definition

An identifier is a sequence of characters that is used to name variables, methods, classes, modules, and other entities.
In S++, there are two types of identifiers: type identifiers and non-type identifiers. They have different syntactic
rules, allowing for clear identification of each, and simpler parsing.

These 2 diferent types of identifiers have different lexeme rules, meaning that they are differientiated at the parser (
the syntactic level)

## Naming rules

| Type     | Rule                | Example                        |
|----------|---------------------|--------------------------------|
| Non-Type | `[a-z][_a-z0-9]*`   | `foo`, `bar`, `baz`, `foo_bar` |
| Type     | `[A-Z][a-zA-Z0-9]*` | `Foo`, `Bar`, `Baz`, `FooBar`  |

Identifiers are case-sensitive, meaning that `Foobar` and `FooBar` are different types. Variable identifiers don't
suffer from any case-sensitive ambiguity, as they cannot contain capital letters.

S++ distinguishes between variable and type identifiers at the parser level, by requiring type identifiers to start with
an uppercase character, such as `Str` and `U32`. Following the first (uppercase) character, they can contain any
alphanumeric character. This forces `PascalCase` to be used.

None-type identifiers (variables, methods etc) must start with a lowercase character, and can only contain lowercase
characters, numbers and underscores, forcing `snake_case` to be used.

Forcing all types to use `PascalCase`, and other identifiers to use `snake_case`, code across multiple libraries will
always follow the same identifier conventions, meaning code will always be consistent and remain readable.