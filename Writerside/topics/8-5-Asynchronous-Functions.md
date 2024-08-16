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
> {style="note"}

### Return Value Wrapping

Any function called with the `async` modifier, will immediately return a `Fut[T]` type, where `T` is the original return
type of the function. `let x = async function_name()` will return a `Fut[T]` type, where the internal value will resolve
in the background. There is an `await` method that can be called on the `Fut[T]` type to block the current thread until
the value is resolved, but alternatively, `function_name` could have just been called synchronously.

## Borrowed Parameters in the Asynchronous Context

As issue that arises with asynchronous function is the memory model. With synchronous functions, the line following a
function call will only execute after the function call frame is popped. With an asynchronous function, this is not
possible, because there is no guarantee that the function call frame will be popped before the next line is executed.

See the section on [pinning](11-5-Pinning.md) for more information on how S++ handles this issue.
