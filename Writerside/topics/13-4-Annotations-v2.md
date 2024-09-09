# 13.4. Annotations v2

## Annotations

### Builtin Annotations

There are a number of builtin annotations that either map to llvm function attributes for compiler optimizations, or
provide further information to the S++ compiler.

**Exposed**
- **`@inline(how: InlineLevel)`**: Maps to one of the `llvm` inline function attributes. This attribute
  specifies how the function should be inlined. The `how` parameter must be one of the following:
    - `InlineLevel::None`: No inlining is done: maps to `noinline`.
    - `InlineLevel::Optimize`: Inlining is done at the optimization level: maps to `inlinehint`.
    - `InlineLevel::Always`: Inlining is always done: maps to `alwaysinline`.
- **`@cold()`**: Maps to the `llvm::cold` function attribute. This attribute is used to specify that the function
  is rarely executed.
- **`@hot()`**: Maps to the `llvm::hot` function attribute. This attribute is used to specify that the function
  is frequently executed.

**Internal**
- **`@memory(mem: MemRule, arg_mem: MemRule, ina_mem: MemRule)`**: Maps to the `memory` function attribute. This
  annotation is not available to the user, but is used by the S++ compiler following symbol table analysis per function.

### Custom Annotations