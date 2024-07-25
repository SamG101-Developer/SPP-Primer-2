# 8.5. Asynchronous Functions

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

<secondary-label ref="doc-subj-update"/>

## Asynchronous Model

Common implementations of asynchronous function models, such as in Python, Rust, and JavaScript, all require the `async`
keyword at the function definition. This introduces the "function color problem", which causes difficulty and
integration issues when using asynchronous functions in a synchronous context.

To mitigate this, S++ draws inspiration from Golang's `go` keyword and introduces the `async` keyword as a function call
modifier. This allows the function to be executed asynchronously without changing the function definition.

```
let x = async function_name()
```

> Calling a function with the `async` modifier will wrap the return type in a `Fut[T]` type, where `T` is the original
> return type of the function.

{style="note"}

## Borrowed Parameters in the Asynchronous Context

<secondary-label ref="feature-subj-change"/>

As issue that arises with asynchronous function is the memory model. With synchronous functions, the line following a
function call will only execute after the function call frame is popped. With an asynchronous function, this is not
possible, because there is no guarantee that the function call frame will be popped before the next line is executed.

This means that an owned object might be consumed in the outer frame whilst a borrow is active in the asynchronous
function call. To mitigate this potential memory issue, borrows cannot be taken in the asynchronous context.

> Alternative implementations are being considered to allow borrows in the asynchronous context, without violating the
> memory model.

{style="warning"}
