# 3.16. Postfix Member Access

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Postfix Member Access

Postfix member access is used to access a member of an object, namespace, or type. The `.` operator is used for runtime
access, and the `::` operator is used for static access. Neither of these operators can be overloaded.

## Resolving Ambiguous Attribute Access

If multiple super-classes have the same attributes, attempting to access the attribute of a class can be ambiguous. It
is only ambiguous, if more than 1 superclass, on the same level in the inheritance tree, have that attribute name, and
no superclasses closer to the subclass override this name. Example:

```
cls A {
    x: U32
}

cls B {
    x: U64
}


cls C {}
sup C ext A {}
sup C ext B {}


fun main() -> Void {
    let c = C()
    std::print(c.x)
}
```

This is an error, because both `A` and `B` are at the same level in the inheritance tree (+1), and both have an
attribute `x`. If the `C` class had an attribute `x`, then it would be resolved to that attribute.

Whilst C++ allows mixing static access into runtime access, like `c.A::x`, this is not allowed in S++, as member access
strictly goes in the order `static -> runtime`. Also, it isn't needed, as there are `std::` functions for casting. The
correct S++ syntax for this would be:

```
fun main() -> Void {
    let c = C()
    std::print(std::upcast[A](c).x)
}
```

