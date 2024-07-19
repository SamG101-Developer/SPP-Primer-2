# 7.1. Class Definition

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Class System

S++ has a unique class system, inspired by Rust, but simplified and more flexible. Classes have different blocks for
state and for behaviour. A class's state is defined by a `cls` block, which can provide an identifier, generics and
attributes. A `sup` block defined the behaviour of a class - its methods, type aliases, and inheritance.

## Defining a Class

Classes are defined by the `cls` keyword, followed by the class name. The class name must be a valid type identifier.
Generic parameters and constraints can follow, and the class body is enclosed in curly braces. The class body only
defined the state of the class, and can therefore only contain attribute identifiers and type annotations.

An example of a class definition is as follows:

```
cls Vec3D[T: Copy] {
    x: T
    y: T
    z: T
}
```

## Forward Declarations

Forward declarations are not required in S++, and therefore have no syntax. This is because the compiler performs
multi-state semantic analysis, and resolves all types before they are used, by filling the symbol tables with symbols
generated from module-level blocks. This means that a class can be used before it is defined, and the compiler will
resolve the class correctly. Defining a type more than once will result in a compiler error.
