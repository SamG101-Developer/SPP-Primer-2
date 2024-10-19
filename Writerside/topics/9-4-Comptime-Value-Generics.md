# 9.4. Comptime Value Generics

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Compile-Time Value Generics

Like C++ and Rust, S++ supports compile-time value generics. This is useful for defining arrays, and other compile-time
constants that need to be generic.

```
cls Array[T, cmp n: U64] { }
```

## Flexibility

Compile-time constants are not limited to numbers, booleans and chars. Any expression that can be used as
a [global constant](4-3-Global-Constants.md)'s value, can be used as a compile-time generic argument. This is the same
as C++ structs that have a `constexpr` constructor with an argument per attribute; because the S++ universal object
initialization initializes every variable, the same can be done with compile-time constants, given the arguments
themselves are all compile-time constants.


```
cls DataType {
    x: BigDec
    y: BigDec
}


fun function[cmp n: DataType](b: Bool) -> Void {
    ...
}


fun main() -> Void {
    function[DataType(x=1, y=2)](true);
}
```
