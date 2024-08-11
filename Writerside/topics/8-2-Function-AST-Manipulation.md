# 8.2. Function AST Manipulation

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Function AST Manipulation

Function AST Manipulation is a small but important feature in S++, and is critical to
provide [overloading](8-3-Function-Overloading.md) callable types in S++. Because functions can either be defined with
standard `fun` syntax or by actually superimposing a `Fun___`type over a class, all `fun` functions are translated
to `Fun___` superimpositions, and then analysed:

```
fun foo(a: Str) -> Void { std::print("Str") }
fun foo(a: U32) -> Void { std::print("Num") }
```

```
cls __Mock_foo { }

sup FunRef[Void, (Str)] on __Mock_foo {
    fun call_ref(&self, a: Str) -> Void {
        std::print("Str")
    }
}

sup FunRef[Void, (U32)] on __Mock_foo {
    fun call_ref(&self, a: U32) -> Void {
        std::print("Num")
    }
}

let foo = __Mock_foo()
```

This allows [overloading](8-3-Function-Overloading.md) to be implemented in a way that is consistent with the rest of
the language, and allows for functions to be passed around as first-class citizens. This is a critical feature for a
language that is designed to be functional, and allows for a lot of flexibility in how functions are used in S++.
