# 9.2. Generic Constraints

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Generic Constraints

S++ provides shorthand and longhand ways to define generic constraints. The shorthand way is to use the `:` token after
a generic, followed by the generic constraints. The longhand way is to use the `where` keyword, followed by a list of
generic constraints.

Because intersection types are supported, multiple constraints can be defined as 1 constraint, by defining an
intersection type as the constraint.

### Shorthand

```
fun function[T: Add, Sub](a: T, b: T) -> Void {
    let x = a + b;
    let y = a - b;
}
```

The generic type argument `T` is constrained to be both `Add` and `Sub`. This means that both `Add` and `Sub` must be
superimposed over the type `T`.

### Longhand

```
fun function[T](a: T, b: T) -> Void where [T: Add, Sub] {
    let x = a + b;
    let y = a - b;
}
```

Multiple generic types can be constrained to the same constraints, by doing `where [T, U: Copy]` for example. Each
generic can only be constrained once, for simplicity reasons. Therefore, using shorthand and longhand for 1 generic
type, or multiple longhand constraints for the same generic type, is not allowed.

## Comparison to C++ Concepts

C++
:
```++
template <typename T>
concept Hashable = requires(T a) {
    { std::hash<T>{}(a) } -> std::converitable_to<std::size_t>;
};
```
```++
template <Hashable T>
void f(T a) {}
```

S++
:
```
fun f[T: Hash](a: T) -> Void {}
```
This can logically be interpreted as constraining `T` to `Hash`, and constraining `a` to `T`. Or, `T` is a type of
`Hash`, and `a` is a type of `T`.
