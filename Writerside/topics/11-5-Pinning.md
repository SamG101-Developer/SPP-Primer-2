# 11.5. Pinning

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

## Pinning

Pinning a value means that the value is not allowed to move in memory. The `pin` keyword marks a symbol a "pinned", and
during memory integrity checks, it ensures that a move or partial move cannot take place from a pinned value. This is
required in allowing borrows to be used in async function calls or coroutines, as the value must be guaranteed to not
move in memory.

A pin is made using the "pin" keyword, followed by the symbol to pin. As a symbol is pinned, the operand must be a
variable or attribute. The syntax is:

```
pin symbol
```

## Async Functions & Coroutines

Async functions and coroutines cannot accept normal borrows as a synchronous subroutine would.

For async functions, it isn't possible to guarantee that a borrowed argument will remain valid for the entirety of the
function's execution; the corresponding owned object could be consumed in the caller, leading to undefined behaviour in
the asynchronous function.

For coroutines, the same issue arises, but between suspension points. This is because a borrowed value could be passed
in, the coroutine suspends, the owned object is consumed in the caller context, then the coroutine attempts to use the
borrow again, leading to undefined behaviour.

Therefore, a mechanism is required to ensure that the borrow remains valid for the duration of the asynchronous function
or coroutines. The key issue is that the exit point for these calls is non-deterministic, meaning standard analysis
isn't possible. The "pinning" model is used to guarantee that borrows remain valid for the duration of the call.

A borrow is "pinned", if the corresponding owned object that the borrow points to is pinned. If all borrows into a
coroutine of asynchronous function are pinned, then it is guaranteed that they are valid for the duration for the
entire non-deterministic duration of the call.

There are a few restrictions to ensure that borrowed object's lifetimes remain valid within an asynchronous function
call or a coroutine:
1. A borrowed value's corresponding owned object must be pinned. This ensures that the borrowed value is valid for the
   duration of the call, and the memory location it points to will not move.
2. Unpinning a value that currently has a pin target (coroutine or async function call) will result in the pin target
   being inaccessible it will be marked as moved and therefore unusable.
3. For a function to return, all pins must be resolved. This prevents a coroutine from being returned, when it is
   borrowing from a value inside the function.

On top of variable being able to be pinned, parts of objects can be pinned too. This allows for a more fine-grained
control of what can't move. This is the same as borrowing part of an object for a function call.

As with every branch of a `case` expression having to leave symbols in the same memory state regarding ownership, the
same applies to the "pin" state of symbols. This ensures uniform behaviour after the `case` expression has been
executed.

## Example with Coroutines

In the following example, a coroutine is defined that borrows a value from the stack. The value is pinned, and then
passed to the coroutine. The coroutine can then use the borrowed value, as it is guaranteed to not move in memory.

```
cor test_coroutine(a: &Str) -> GenMov[Gen=Str] {
    len = a.length()
    
    gen "a" * len
    gen "b" * len
}

fun main() -> Void {
    let a = "random_string"
    pin a
    
    coro = test_coroutine(&a)
    loop case coro then is Str {
        std::print(coro)
    }
}
```
