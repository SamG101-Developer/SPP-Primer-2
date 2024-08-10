# 8.6. Partial Functions

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Partial Functions

Partial functions syntactic sugar for lambdas, with captures whose conventions matches the corresponding usage as
arguments. The `_` token is used to mark missing parameters, and the `..` token can be used to mark missing variadic
arguments.

## Partial Function Example

```
fun function(a: Str, b: Str, c: Str, d: &Str) -> Str { }

fun main() -> Void {
    let a = "hello"
    let d = "hello"
    
    let p = function(a, _, _, &d)
}
```

This is transformed into:

```
fun main() -> Void {
    let a = "hello"
    let d = "hello"
    
    let p = fun (b: Str, c: Str) [a, &d] {
        function(a, b, c, d)
    }
}
```

## Optional Parameters

```
fun function(a: Str, b: Str, c: Str = "default") -> Str { }

fun main() -> Void {
    let a = "hello"
    let p = function(a, _)
}
```

**TODO**

