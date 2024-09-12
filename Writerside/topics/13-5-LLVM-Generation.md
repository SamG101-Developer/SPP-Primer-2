# 13.5. LLVM Generation

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## LLVM IR

S++ generates LLVM IR code. Modules, classes and functions are translated into LLVM IR.

### Module

### Class

### Function

The LLVM syntax for functions is:

```
define [linkage] [PreemptionSpecifier] [visibility] [DLLStorageClass]
       [cconv] [ret attrs]
       <ResultType> @<FunctionName> ([argument list])
       [(unnamed_addr|local_unnamed_addr)] [AddrSpace] [fn Attrs]
       [section "name"] [partition "name"] [comdat [($name)]] [align N]
       [gc] [prefix Constant] [prologue Constant] [personality Constant]
       (!name !N)* { ... }
```

- **Linkage**: The linkage is controlled by decorators; whilst the `@private`/`@protected` marks the symbol as only
  accessible within the module / submodules, and `@public` marks the symbol as accessible outside the module, they also
  map to the llvm linkage specifiers `private` and `external` respectively.
- **PreemptionSpecifier**: The `dso_local` is used for all code local to the library or executable being compiled.
  All `_stub.spp` file function signatures use the `dso_preemptable` specifier, as these symbols are pulled from the
  library being linked.
- **Visibility**: The visibility is also controlled by decorators: the `@private` maps to `hidden`, `@protected` maps to
  `protected`, and `@public` maps to `default`.
- **DLLStorageClass**: The DLL storage class is not used (for now).
- **Calling Convention**: The calling convention is controlled by the argument to the `@cconv` decorator. The available
  arguments must match one from `ccc`, `fastcc`, `coldcc`, `ghccc`, `cc11`, `anyregcc`, `preserve_mostcc`,
  `preserve_allcc`, `preserve_nonecc`, `cxx_fast_tlscc`, `tailcc`, `swiftcc`, `swifttailcc`, `cfguard_checkcc`, `cc`.
  The `n` argument can be used with the `cc` calling convention. A valid example would be `@convcc("fastcc")`.
- **Return Attributes**: Todo
- 
