# 13.4. Annotations

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Annotations

S++ allows annotations of modules, classes, methods, and typedefs. Custom annotations can be applied to methods and
classes only.

### Builtin Annotations

There are a number of builtin annotations that either map to llvm function attributes for compiler optimizations, or
provide further information to the S++ compiler.

#### LLVM Annotations
- **`@inline(how: InlineLevel)`**: Maps to one of the `llvm` inline function attributes. This attribute
  specifies how the function should be inlined. The `how` argument must be one
  from `InlineLevel::None`, `InlineLevel::Optimize`, or
  `InlineLevel::Always`, which map to `noinline`, `inlinehint`, and `alwaysinline` respectively.
- **`@cold()`**: Maps to the `llvm::cold` function attribute. This attribute is used to specify that the function
  is rarely executed.
- **`@hot()`**: Maps to the `llvm::hot` function attribute. This attribute is used to specify that the function
  is frequently executed.

#### Main Module Annotations
- **`@no_std()`**: This marks a project as not using the `std` library. Only the `main.spp` file can use this
  annotation, and it applies to the entire project.
- **`@use_unstable(feature: Str)`**: This allows an unstable feature to be used in the project. Again, it can only be
  used in the `main.pp` file, and it falls through to all modules.

#### Encapsulation Annotations
- **`@public(module: Str)`**: This makes a method, class or module public. This maps to both the visibility and linkage
  attributes in LLVM, as-well as symbol access in S++. A module can be made public to certain other modules with
  the `module` argument.
- **`@protected()/@private`**: This marks a method, class, or module as private/protected. Protected modules are only
  accessible to this module and submodules, where-as private modules can only be accessed by same-namespace modules.
  Linkage and visibility are set accordingly.
- **`@friend[..Friends]()`**: This allows a number of types to be marked as friends of either a module, class, or
  function.

#### Compiler Output Annotations
- **`@deprecated(message: Str)`**: This marks a method, class, or module as deprecated. The `message` argument is a
  string that will be printed when the deprecated item is used.
- **`@obsolete(message: Str)`**: This marks a method, class, or module as obsolete. The `message` argument is a
  string that will be printed when the obsolete item is used.
- **`@warning(message: Str)`**: This marks a method, class, or module as having a warning. The `message` argument is a
  string that will be printed when the warning item is used.

#### Method Annotations
- **`@abstractmethod()`**: This marks a method as abstract, and must be overridden in any subclasses. It also makes the
  containing class abstract, and therefore non-instantiable.
- **`@virtualmethod()`**: This marks a method as virtual, and can be overridden in direct subclasses. It must be
  re-marked as `@virtualmethod` for the next layer of subclasses to be able to override it again.

### Custom Annotations

Custom annotations work like Python annotations. They take a function or a class, and can run code before or after the
function or class is defined. Custom annotations are defined as functions too:

#### Custom Function Annotations

```
@my_decorator(message="Debug")
fun my_decorated_function(a: BigInt, b: BigInt) -> BigInt {
    std::print("Hello, World!")
    ret a + b
}

fun my_decorator[Ret, In](function: FunRef[Ret, In], name: Str) -> FunRef[Ret, In] {
    fun wrapper(args: In) -> Ret {
        std::print("Decorating function: " + message)
        ret function(..args)
    }
    ret wrapper
}
```

This transforms `my_decorated_function(100, 200)` to `my_decorator(my_decorated_function, message="Debug")(100, 200)`.
