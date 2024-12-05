# 14.7. Option &amp; Result

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Option

### Option Definition

The `Opt[T]` type is a type that represents an optional value. It is either `Some[T]` or `None`. The `Some[T]` variant
contains a value of type `T`, and the `None` variant contains no value. The option type is defined as follows:

```
cls None { }
cls Some[T] { value: T }
use Opt[T] = Some[T] | None
```

### Option Usage

The type union is used, so that destructuring can be performed on the option type, rather than having to rely on using
utility methods such as `is_some` and `is_none`. These utility methods are still available, but are not the primary way
to check the value of an option.

An option is typically checked using a `case` expression, as shown below:

```
case option of
    is Some(value) { }
    is None { }
```

It can be seen that the `Some` type is destructured, and the `value` attribute is extracted. Flow typing will then treat
the `value` variable with the type `T` (generic type argument of the option type). Either `None` or `else` can be used,
as there are only 2 types forming the `Opt[T]` type, and the `None` type doesn't contain any attributes.

## Result

### Result Definition

The `Res[T, E]` type is a type that represents either a successful value of type `T`, or an error of type `E`. These
types are wrapped in `Pass` and `Fail` variants respectively. The result type is defined as follows:

```
cls Pass[T] { value: T }
cls Fail[E] { error: E }
use Res[T, E] = Pass[T] | Fail[E]
```

### Result Usage

```
case result of
    is Pass(value) { }
    is Fail(error) { }
```

## Design Decisions

- **Defining residual types as unions** rather than classes allows them to be neatly destructured.
- **Using `case opt is Some(value)`** rather than `case let Some(value) = opt` is a design decision to keep the
  syntax consistent with other pattern matching expressions.
