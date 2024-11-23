# 2.8. Tokens

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

<secondary-label ref="doc-subj-update"/>

## Tokens

- `{`
- `}`
- `[`
- `]`
- `(`
- `)`
- `:`
- `_`
- `..`
- `?`
- `.`
- `->`
- `@`
- `+`
- `-`
- `*`
- `/`
- `%`
- `**`
- `%%`
- `&`
- `|`
- `<=>`
- `==`
- `!=`
- `>`
- `<`
- `>=`
- `<=`
- `=`
- `+=`
- `-=`
- `*=`
- `/=`
- `%=`
- `**=`
- `%%=`
- `??`
- `,`

{columns="4"}

## Token Descriptions

### Brackets

`{ }`
: - Introduce a new scope (`cls/sup/fun/case/loop/with/else` or unnamed).

`[ ]`
: - Group generic parameters or arguments.
: - Group long-hand generic constraints (`where`)
: - Group lambda captures.

`( )`
: - Group function parameters or arguments.
: - Group object initialization arguments.
: - Group destructure operation values.
: - Group multiple type imports.
: - Parenthesized expressions.
: - Tuple literals.

### Arithmetic Operators

`+`
: - Define a positive number literal.
: - Define a binary addition operation.

`-`
: - Define a negative number literal.
: - Define a binary subtraction operation.

`*`
: - Define a binary multiplication operation.

`/`
: - Define a binary division operation.

`%`
: - Define a binary remainder operation.

`**`
: - Define a binary exponentiation operation.

`%%`
: - Define a binary modulo operation.

### Type Operators

`&`
: - Define a borrow operation.
: - Define an intersection type.

`|`
: - Define a union type.

### Comparison Operators

`<=>`
: - Define a comparison operation.

`==`
: - Define an equality operation.

`!=`
: - Define an inequality operation.

`>`
: - Define a greater-than operation.

`<`
: - Define a less-than operation.

`>=`
: - Define a greater-than-or-equal operation.

`<=`
: - Define a less-than-or-equal operation.

`=`
: - Define a named argument or generic type argument.
: - Define a default value for an optional parameter or generic type parameter.
: - Define a binding operation within a destructuring operation.
: - Define a binding for the `with` block.
: - Define an initialized variable or an assignment operation.

### Compound Assignment Operators

`+=`
: - Define an addition assignment operation.

`-=`
: - Define a subtraction assignment operation.

`*=`
: - Define a multiplication assignment operation.

`/=`
: - Define a division assignment operation.

`%=`
: - Define a remainder assignment operation.

`**=`
: - Define an exponentiation assignment operation.

`%%=`
: - Define a modulo assignment operation.

### Residual Operators

`?`
: - Propagate an error to the caller for a residual type in a function.

`??`
: - Define a null coalescing operation.

### Other Operators

`,`
: - Separate items in a list of values.

`:`
: - Define a type annotation
: - Define a generic type constraint.

`_`
: - Skip single values in a destructure operation.
: - Skip single parameters in a function call, creating a partial function application.

`..`
: - Define a variadic parameter in a function signature (last parameter).
: - Define a variadic generic type parameter (last generic type parameter).
: - Tuple unpacking (tuple into multiple arguments).
: - Binary or function folding operation.
: - Bind multiple elements from a tuple into a new tuple variable.
: - Skip multiple elements in a tuple destructure operation.

`.`
: - Runtime member access of an object.
: - Decimal point in a floating-point number.

`->`
: - Define a function return type.
: - Define a lambda return type.

`@`
: - Define an annotation.
