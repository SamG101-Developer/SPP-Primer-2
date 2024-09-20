# 7.5. Inheritance &amp; Polymorphism

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Method Overriding

In order to override a method, the base method must be marked as either virtual, or abstract, using the `@virtualmethod`
or `abstractmethod` annotations respectively. There is no need for an "override" annotation, because all methods
in `sup-on` blocks are overrides.

Methods not marked as virtual or abstract are "final", and cannot be overridden. There is no "final" annotation, because
it is the default.

```
sup BaseClass {
    @virtualmethod
    fun method(&self) -> Void { }
    
    @abstractmethod
    fun abstract_method(&self) -> Void { }
}

sup DerivedClass ext BaseClass {
    fun method(&self) -> Void { }   
    fun abstract_method(&self) -> Void { }
}
```

Using the `@abstractmethod` to mark a method as abstract automatically marks the class as abstract, and therefore
non-instantiable. This also means that the class cannot have any attributes, otherwise it would need to be instantiated
for subclasses' `sup=` argument.

The `BaseClass` type is abstract, and cannot be instantiated. The `DerivedClass` type is concrete, and can be
instantiated, and because `BaseClass` is abstract, and has not attributes, it is state;ess, and doesn't have to appear
in the `sup=` argument.
