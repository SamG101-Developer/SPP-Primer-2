# 7.6. Encapsulation &amp; Access Modifiers

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Access Modifiers

There are 4 access modified in S++: `@public()`, `@protected()`, `@packaged` and `@private()`. These are used to control
the visibility of classes, attributes, methods, typedefs and modules. The default access modifier to these items are
`@protected` for module members and `@private()` for class members.

| Access Modifier | Module Member                                                               | Class / Superimposition Member                   |
|-----------------|-----------------------------------------------------------------------------|--------------------------------------------------|
| `@private()`    | Member visible to the entire module, **not submodules**.                    | Member visible to the class, **not subclasses**. |
| `@protected()`  | Member visible to the entire module, and all submodules.                    | Member visible to the class and all subclasses.  |
| `@public()`     | Member visible to the entire module, all submodules, and all other modules. | Member visible to all classes / functions.       |

Annotations can be applied to the following items:
- Module members: classes, typedefs, functions and global constants.
- Class/superimposition members: attributes, methods and typedefs.
