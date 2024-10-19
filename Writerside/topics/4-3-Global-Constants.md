# 4.3. Global Constants

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Global Constants

S++ supports global constants, that are defined at the module level, and accessible via the namespace of the module.
Standard visibility annotations apply to them. Global constants are readonly, and therefore must be initialized at
definition by a compile-time expression.

Global constants are auto-pinned and are can not be unpinned, allowing the compiler to know that the symbol's contents
will remain in the same place in memory for the duration of the program. These are synonymous to Rust's `const`
expressions.

Global constants cannot be redefined in the same module. They cannot be moved from (but can be copied if they
superimpose `Copy`). This makes them good replacements of traditional macros in other languages.

### Compile-Time Expressions

A "compile-time expression" is an expression whose value can be determined without running the code. This is currently
limited to literals, and object instantiations whose arguments' values are literals (or nested object instantiations).

With future support of compile-time functions, these will also be allowed to be used as values. Object instantiations
are allowed because the universal object initialization syntax only sets the values of all attributes, meaning that it
can be guaranteed that the object will be generated as it appears.

## Example

```
cmp x: std::BigInt = 123
cmp y: std::String = 456
cmp p: Point(x=100, y=200)
```
