# 3.6. Case Expressions

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Example: Basic Branching Expression

The following example show a basic usage of the `case` expression, which is similar, if not identical, to most other
languages' `if` statements/expressions. The standard branches are `case`, `else case` and `else`.

```
let result = case some_condition {
    "hello world"
}
else case some_other_condition_1 {
    "hello galaxy"
}
else case some_other_condition_2 {
    "hello universe"
}
else {
    "hello multiverse"
}
```

## Example: Imitation Ternary Operator

Most languages use `?:` as the ternary operator - in S++, the keywords `case` and `else` are used, in an "inline" form.
This follows the left-to-right convention of the language, and is more readable than the `?:` operator.

```
let result = case some_condition { "true" } else { "false" }
```

## Example: Partial Fragments (Expressions)

To simplify the comparisons of variables, partial fragments can be combined with pattern fragments, to generate multiple
expressions that re-use the partial fragment. This works, because all comparison operators use the `&self` parameter in
the appropriate function call of the operator class.

The comparison operators `==`, `!=`, `<`, `>`, `<=`, `>=` and `is` can be used for the combination of partial fragments
and pattern fragments. The `is` operator is discussed later; this section focuses on the other comparison operators.

The `of` keyword following the partial fragment is used to separate the partial fragment from the pattern fragments.
This is important, as the partial fragment is used in combination with multiple pattern fragments. Non-multi-pattern
cases don't require the `of` keyword (as seen above).

This is required, as with more complex operators, it wouldn't be possible to determine where to split the binary
expression: in `case expression == 123 == other == 456`, there are 3 places to split the expression.

```
case person of
    == john { "hello john" }
    == jane { "hello jane" }
else { "hello stranger" }
```
```
case age of
    <= -1 { "error" }
    == 00 { "newborn" }
    <  18 { "child" }
    <  60 { "adult" }
else { "senior" }
```

## Example: Multiple Pattern Fragments

```
case person of
    == john, jane { "hello john or jane" }
    == jack, jill { "hello jack or jill" }
else { "hello stranger" }
```

> This is only allowed for non-`is` destructures, so that only 1 set of variables are considered. This might expand into
> allowing multiple patterns as long as the same variables are introduced by each pattern.
> {style="warning"}

## Example: Destructure Pattern (Array)

```
case array of
    is [1, ..] { "array starts with 1" }
    is [2, ..] { "array starts with 2" }
else { "tuple is something else" }
```

> There must be the same number of attributes on the lhs as the rhs, unless the `..` token is used, which skips the rest
> of the missing arguments. The `..` token can appear anywhere in the pattern, but can only appear once.
> {style="warning"}

## Example: Destructure Pattern (Tuple)

```
case tuple of
    is (1, ..) { "tuple starts with 1" }
    is (2, ..) { "tuple starts with 2" }
else { "tuple is something else" }
```

> There must be the same number of attributes on the lhs as the rhs, unless the `..` token is used, which skips the rest
> of the missing arguments. The `..` token can appear anywhere in the pattern, but can only appear once.
> {style="warning"}

## Example: Destructure Pattern (Object)

```
case person of
    is Person(name="john", ..) { "hello john" }
    is Person(name="jane", ..) { "hello jane" }
else { "hello stranger" }
```

> All attributes must be present, unless the `..` token is used, which skips the rest of the attributes. This follows
> regular object destructuring rules.
> {style="warning"}

> The `is` destructure also allows for variant types to be directly considered, combined with flow typing. This is
> shown in the below example.
> {style="note"}

## Example: Destructure Pattern (Variant Types)

```
case optional_string of
    is Some(val) { "string is ${val}" }
    is None { "string is None" }
```

## Example: Destructure Pattern with Bindings

```
case tuple of
    is (a, 1) { "tuple is ($a, 1)" }
    is (b, 2) { "tuple is ($b, 2)" }
else { "tuple is something else" }
```
```
case array of
    is [a, 1] { "array is [$a, 1]" }
    is [b, 2] { "array is [$b, 2]" }
else { "array is something else" }
```
```
case person of
    is Person(name="john", age, ..) { "hello john, you are ${age}" }
    is Person(name="jane", age, ..) { "hello jane, you are ${age}" }
else { "hello stranger" }
```

## Example: Pattern Guards on Destructures

```
case person of
    is Person(name="john", age, ..) and age < 18 { "hello john, you are a child" }
    is Person(name="john", age, ..) and age < 60 { "hello john, you are an adult" }
    is Person(name="john", age, ..) { "hello john, you are a senior" }
else { "hello stranger" }
```
