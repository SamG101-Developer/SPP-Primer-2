# 7.6. Encapsulation &amp; Access Modifiers

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Access Modifiers

There are 3 access modified in S++: `@public()`, `@protected()`, and `@private()`. These are used to control the
visibility of classes, attributes, methods, typedefs and modules. The default access modifier to these items
are `@private()`. None of these decorators take an argument.

The `@friend()` decorator is used to allow a class to access the private and protected classes, functions, and modules.
For example, using `@friend[A, B, C]()` on type `D` would allow `A`, `B`, and `C` to access the private and protected
members of `D`.
