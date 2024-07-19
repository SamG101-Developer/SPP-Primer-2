# 7.2. Superimposition

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

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
