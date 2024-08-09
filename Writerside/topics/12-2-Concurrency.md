# 12.2. Concurrency

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Concurrency

S++ uses coroutines as the primary concurrency mechanism. They allow for a function to be suspended and resumed at a
later time, with bidirectional communication between the caller and the coroutine. They are defined using the standard
[function](8-1-Function-Definition.md#function-definition), and using the `cor` keyword, not `fun`. A coroutine will
typically include `gen` expression - generating a value is the same as other language's "yielding" values.

### Coroutine Return Type

<secondary-label ref="feature-frozen"/>

<secondary-label ref="doc-sect-complete"/>

A coroutine's return type must be one of the following three available types:

1. `GenMov[Gen, Ret, Send]`
2. `GenMut[Gen, Ret, Send]`
3. `GenRef[Gen, Ret, Send]`

Each generator type corresponds to the convention being used with the `gen` expressions inside the coroutine. For
example, if values are being yielded with the `&mut` convention, then the `GenMut` return type must be used. All `gen`
expressions within a coroutine must use the same convention.

### Passing Data Out of a Coroutine

<secondary-label ref="examples-todo"/>

The `gen` expression is used to yield values out of a coroutine. Due
to [second-class borrows](11-3-Second-Class-Borrows.md), borrows can be yielded out of a coroutine. The syntax is to
just add the convention between the `gen` keyword and the value to be yielded: `gen &value`. The type of the object
being yielded must match the `Gen` generic parameters corresponding generic argument. Matching borrows against the
coroutine's full return type is a strict requirement, as described in the section above.

Being able to yield borrows out of a coroutine is a powerful feature, as it allows for coroutines to be used as the
[iterator]() model. The three iterator classes each map to the corresponding generator type, and the `gen` expression
is used to yield the next value in the sequence.

If the `Gen` generic parameter's argument is `Void`, then the `GenMov` return type must be used. This is because it is
impossible to borrow a `Void` value, as it is a zero-sized type.

### Passing Data Into a Coroutine

<secondary-label ref="examples-todo"/>

Data can be sent into a coroutine, and received by modifying the `gen` expression with a `let` declaration. The
statement `let var = gen 123` will yield `123`, suspending the coroutine. Once the coroutine is resumed,
with `coroutine.next("hello")`, the value `"hello"` will be sent into the coroutine, and the assigned to the
variable `var`. The type of value being sent in must match the `Send` generic parameter's corresponding generic
argument.

Because generic function parameters that become `Void` are simply removed from function signatures, if the `Send`
generic parameter's argument is `Void`, then the `next` method is just called without any arguments: `coroutine.next()`.

### Conflict with Memory Rules

<secondary-label ref="feature-frozen"/>

All objects being sent into coroutines must be owned objects, because the coroutine could suspend, the caller consume
the owned object, then the coroutine resume and attempt to use the borrow. For the same reason, all variables passed to
the coroutine functions as function arguments must be owned objects. It is checked that all parameters use the move
convention, when the coroutine prototype is defined.

### Coroutine Chaining

<secondary-label ref="doc-sect-wip"/>

<secondary-label ref="doc-sect-subj-update"/>

<secondary-label ref="examples-todo"/>

Coroutine chaining is a feature taken from Python, which allows for a coroutine to call another coroutine, and yield all
its values out in order. This is done by using the `gen with` expression, which just maps to a loop that yields all the
values from the inner coroutine. The inner coroutine must have a return type that is compatible with the outer
coroutine.
