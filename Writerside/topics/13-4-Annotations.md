# 13.4. Annotations

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Annotations

Annotations can be provided to modules, classes and functions. They will only be enabled once the compiler is
self-hosting. This is because they work like a mix of Python decorators, and Rust attributes. They are defined as
functions that accept an AST, and allow modification of it. A new layer in the compiler will be added for it - before
the preprocessing stage.

Annotations are method calls. They must always include `()` even if there are no arguments. This is different from
Python, which doesn't require the `()` on decorators.

This is a list of compiler-builtin annotations, and the context they are used in:

| Annotation                             | Module Context | Class Context | Function Context |
|----------------------------------------|----------------|---------------|------------------|
| `@no_std()`                            | ✓              | ✕             | ✕                |
| `@allow_unstable(..features)`          | ✓              | ✕             | ✕                |
| `@public(module="*")`                  | ✓              | ✓             | ✓                |
| `@protected()`                         | ✓              | ✓             | ✓                |
| `@private()`                           | ✓              | ✓             | ✓                |
| `@final()`                             | ✕              | ✓             | ✓                |
| `@abstractmethod()`                    | ✕              | ✕             | ✓                |
| `@virtualmethod()`                     | ✕              | ✕             | ✓                |
| `@deprecated(message="")`              | ✓              | ✓             | ✓                |
| `@obsolete(message="")`                | ✓              | ✓             | ✓                |
| `@warning(message="")`                 | ✓              | ✓             | ✓                |
| `@inline(level=InlineLevel::Optimize)` | ✕              | ✕             | ✓                |
| `@rare()`                              | ✕              | ✕             | ✓                |                                     

## Custom Annotations

Custom annotations can be provided too. Their syntax is a function, whose first parameter is either
a `&mut ModulePrototypeAst`, `&mut ClassPrototypeAst`, or `&mut FunctionPrototypeAst`, followed by any other arguments.
The function can be defined in any namespace, as annotations can be postfix-type-accessed, ie `@std::some_annotation()`.

## Module annotations

Module annotations must be placed before the `mod` keyword. They are defined as functions that accept
an `&mut ModulePrototypeAst`, and any following arguments.

```
@std::allow_unstable("allocator")
@std::allow_unstable("generator")
mod my_library::my_module
```

```
fun allow_unstable(ast: &mut ModulePrototypeAst, ..names: Str) -> Void { }
```

## Class annotations

Class annotations must be placed before the `cls` keyword. They are defined as functions that accept
an `&mut ClassPrototypeAst`, and any following arguments.

```
@std::public(module="testing")
@std::final()
cls MyType
```

```
fun public(ast: &mut ClassPrototypeAst, module: Str) -> Void { }
fun final(ast: &mut ClassPrototypeAst) -> Void { }
```
