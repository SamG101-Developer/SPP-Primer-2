# 7.5. Inheritance &amp; Polymorphism

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Controlled Inheritance

Whilst S++ supports object-oriented programming, and encourage its use, it also controls inheritance very tightly.
Features that assist with this include and are limited to:
- superimposition-based inheritance - split each inheritance of a type into a separate block
- the `Castable[T]` type used for values which will be upcast or downcast

## Method Overriding

In order to override a method, the base method must be marked as either virtual, or abstract, using the `@virtualmethod`
or `abstractmethod` annotations respectively. There is no need for an "override" annotation, because all methods
in `sup-on` blocks are overrides.

Methods not marked as virtual or abstract are "final", and cannot be overridden. There is no "final" annotation, because
it is the default.

```
sup BaseClass {
    @virtual_method
    fun method(&self) -> Void { }
    
    @abstract_method
    fun abstract_method(&self) -> Void { }
}

sup DerivedClass ext BaseClass {
    fun method(&self) -> Void { }   
    fun abstract_method(&self) -> Void { }
}
```

Using the `@abstract_method` to mark a method as abstract automatically marks the class as abstract, and therefore
non-instantiable (except for [base class initialization](7-3-Object-Initialization.md#superclasses)).