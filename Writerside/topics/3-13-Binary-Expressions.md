# 3.13. Binary Expressions

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Binary Expressions

Binary expressions are expressions that involve two operands and an operator. The operator is placed between the two
operands, and the expression is evaluated based on the precedence and associativity of the operator. Each binary
expression is converted into a method call.

There are 2 special operators which don't map to methods, because they are converted into code rather than method calls.
The two operators are the null-coalescing operator `??` and the destructuring assignment operator `is`.

## Special Operators

### Null-Coalescing Operator

The null-coalescing operator `??` is used to provide a default value when a residual-type value is in the `Fail` state.
This is commonly seen in the `Opt[T]` types. The operator is used as follows:

```
let x = value ?? default
```

The code is converted into the following:

```
let x = case value then
    is type(value)::PassState { value }
    is type(value)::FailState { default }
```

### Destructuring Assignment Operator

The destructuring assignment operator `is` is used to destructure a union type into its constituent types. The operator
is used as follows:

```
loop coroutine.next() is Some(value) {
    # code
}
```

The code is converted into the following:

```
loop { case coroutine.next() then is Some(value) { value } } {
    # code
}
```
