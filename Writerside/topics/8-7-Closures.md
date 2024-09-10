# 8.7. Closures

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Closures

S++ supports closures, which are anonymous functions that can capture variables from their surrounding scope. Closures
are defined using the `fun` keyword, followed by a list of arguments, and a block of code. The syntax is similar to
functions, but with a few differences:

- Closures can capture variables from their surrounding scope. This is done using the `with` keyword, followed by a
  list of variables to capture. Variables can be captured with conventions, such as `&` for an immutable borrow.
- Parameters don't have type. They are automatically assigned generic parameters types, and the function is only
  analysed once called with arguments. Parameter conventions are required.
- No return type is given. It is automatically deduced from the return statement. All return statements of the closure
  must return the same type.
- Closures the `|` token to introduce the argument list, so that using a closure as an argument doesn't require
  nested parentheses. This decision was purely aesthetic, as it makes the code more readable.

## Example

```
let vec = Vec::from([1, 2, 3, 4])
let sum = vec.iter_ref().fold((&a, &b) {ret a + b})

let expensive_closure = |num| {
    std::print("Calculating slowly...")
    std::thread::sleep(2000)
    ret num
}
```

## Borrowed Captures

If a variable is captured by borrow, then the borrow must be pinned. This ensures that the borrow is not invalidated
until the closure is no longer needed. Unpinning the borrow's owned object will result in the closure being invalidated.
This is the same as unpinning a value being borrowed by a coroutine.

## Closure Function Type

A closure with no captures is the `FunRef` type. If any variables are captured, then the type mirrors the most
restrictive capture. This means that if there is a capture-y-move, then the `FunMov` type is used. This is because the
value can only be consumed once.
