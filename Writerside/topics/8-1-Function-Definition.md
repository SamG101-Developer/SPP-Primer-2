# 8.1. Function Definition

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Function Definition

<secondary-label ref="feature-frozen"/>

- **Function keyword**: Either the `fun` or `cor` keyword is used to define a function. The `fun` keyword defines a
  subroutine, which is a "normal" function. The `cor` keyword defines a coroutine, which is a function that can be
  paused and resumed.

- **Function identifier**: The name of the function, following
  the [non-type-identifier](2-5-Identifiers.md#naming-rules) rules. Multiple functions in the same scope can have the
  same name - this creates [overloads]() of the function with different signatures.

- **Generic type parameters**: A group of generic parameters can optionally be defined following the identifier, in
  square brackets. Some of these generics may be "inferable" from parameter types, or other generic constraints. See
  the [generics]() section for more information about the order they must be defined in.

- **Function parameters**: The group of function parameters, which can be empty, must be defined following the function
  identifier or generic type parameters. Each parameter is defined by an identifier, and a required type. There are a
  few different types of parameters, which are discussed further down.

- **Return type**: The return type of a function must be provided in the function signature, following the function
  parameters and the `->` token. The return type can be any type, but must be present, unlike Rust. The `Void` type must
  be used for a function that doesn't return a value.

- **Where block**: A `where` block can optionally be defined following the function signature, which can be used to
  define constraints on the generic type parameters. This block is not required, but can be useful for defining
  longer sets of more complex constraints that would be difficult to read inline.

- **Function body**: The function body is defined following the function signature or `where` block, and is enclosed in
  curly braces. The body can contain any number of statements, expressions, and control flow constructs. Return
  statements must be present for a function whose return type is not `Void`.
