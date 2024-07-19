# 7.2. Superimposition Definition

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Superimposition Definition

<secondary-label ref="feature-frozen"/>

- **Superimposition keyword**: The `sup` keyword is used to define a superimposition. This is how to superimpose
  methods, typedefs, or entire classes over a type.

- **Generic type parameters**: A group of generic parameters can optionally be defined following the identifier, in
  square brackets. **None** of these generics can be "inferable". See the generics section for more information about
  the order they must be defined in.

- **Super class + on keyword**: If the superimposition is an inheritance superimposition, then the `SuperClass on`
  syntax is required. This tells the compiler which superclass to superimpose over the type. If the superimposition is
  a normal superimposition (methods, typedefs), then this part is not required.

- **Type identifier**: The name of the type being superimposed over, following
  the [type-identifier](2-5-Identifiers.md#naming-rules) rules. Note that this type doesn't have to have been defined by
  a class prototype, but can be a specific generic implementation of a type. For example, `sup Str | Bool` is valid and
  will superimpose methods on the `Var[Str, Bool]` type.

- **Where block**: A `where` block can optionally be defined following the type identifier, which can be used to define
  constraints on the generic type parameters. This block is not required, but can be useful for defining longer sets of
  more complex constraints that would be difficult to read inline.

- **Superimposition body**: The superimposition body is defined following the type identifier or `where` block, and is
  enclosed in curly braces. The body can contain any number of methods or typedefs, but **not** attributes, which are
  state constructs, not behavioural constructs.

## Superimposition

Superimposition is one of the most powerful features of S++. It allows for behaviour to be defined over a class, either
as method and typedefs, or as entire classes. This provides a way to combine classes, whilst keeping the behaviour of
each superclass strictly independent.

There are 2 types of `sup` blocks, mirroring the 2 types of superimposition discussed above:

Normal superimposition:

:
```
sup MyType {
    fun method(&self) -> Void { }
}
```

Inheritance superimposition:

:
```
sup MyType on OtherType {
    fun method(&self) -> Void { }  # Override method
}
```
