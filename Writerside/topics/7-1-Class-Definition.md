# 7.1. Class Definition

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Class Definition

<secondary-label ref="feature-frozen"/>

- **Class keyword**: The `cls` keyword is used to define a class. This is the only way to introduce a class into the
  symbol table - there is no separate struct and class system like in C++.

- **Class identifier**: The name of the class, following the [type-identifier](2-5-Identifiers.md#naming-rules) rules.
  Multiple classes within the same namespace cannot share the same name - this will cause a compile time error to be
  thrown.

- **Generic type parameters**: A group of generic parameters can optionally be defined following the identifier, in
  square brackets. Some of these generics may be "inferable" from attribute types, or other generic constraints. See the
  generics section for more information about the order they must be defined in.

- **Where block**: A `where` block can optionally be defined following the generic type parameters, which can be used to
  define constraints on the generic type parameters. This block is not required, but can be useful for defining
  longer sets of more complex constraints that would be difficult to read inline.

- **Class body**: The class body is defined following the class identifier or `where` block, and is enclosed in curly
  braces. The body can contain any number of attributes, but **not** methods or typedefs, which are behavioural
  constructs, not state constructs.

## Class System

S++ has a unique class system, inspired by Rust, but simplified and more flexible. Classes have different blocks for
state and for behaviour. A class's state is defined by a `cls` block, which can provide an identifier, generics and
attributes. A `sup` block defines the behaviour of a class - its methods, type aliases, and inheritance.
