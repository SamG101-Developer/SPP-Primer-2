# 12.2. Concurrency

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Concurrency

S++ uses coroutines as the primary concurrency mechanism. They allow for a function to be suspended and resumed at a
later time, with bidirectional communication between the caller and the coroutine. They are defined using the standard
[function](8-1-Function-Definition.md#function-definition) syntax, but using the `cor` keyword instead of `fun`. A
coroutine will typically include `gen` expression - generating a value is the same as other languages "yielding" values.

### Coroutine Return Type

<secondary-label ref="feature-frozen"/>
<secondary-label ref="doc-sect-complete"/>

A coroutine's return type must be one of the following three following compiler-known types:

1. `GenMov[Gen, Send=Void]`
2. `GenMut[Gen, Send=Void]`
3. `GenRef[Gen, Send=Void]`

Each generator type corresponds to the convention being used with the `gen` expressions inside the coroutine. For
example, if values are being yielded with the `&mut` convention, then the `GenMut` return type must be used. All `gen`
expressions within a coroutine must use the same convention.

The `Gen` generic type parameter represents the type of the expression being generated in the coroutine;
using `GenMut[Gen=BigInt]` would mean that a valid example `gen` expression would be: `&mut 1`.

The `Send` generic type parameter represents the type of the expression being sent into the coroutine; this is used to
type-infer the `let x = gen expression` statement. If no value are being sent, (ie `Send=Void`), then `let` statements
cannot be used, as `Void` assignment is prohibited. Further to this, the generator's `next` method's parameter is
the `Send` type, so if it is `Void`, no argument is required - void parameters are removed from signatures after generic
conversion.

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

```
cor coroutine_1() -> GenRef[Gen=BigInt] {
    let (x, y, z) = (1, 2, 3)

    gen &x
    gen &y
    gen &z
}
```

### Passing Data Into a Coroutine

<secondary-label ref="examples-todo"/>

Data can be sent into a coroutine, and received by modifying the `gen` expression with a `let` declaration. The
statement `let var = gen 123` will yield `123`, suspending the coroutine. Once the coroutine is resumed,
with `coroutine.next("hello")`, the value `"hello"` will be sent into the coroutine, and the assigned to the
variable `var`. The type of value being sent in must match the `Send` generic parameter's corresponding generic
argument.

Because generic function parameters that become `Void` are simply removed from function signatures, if the `Send`
generic parameter's argument is `Void`, then the `next` method is just called without any arguments: `coroutine.next()`.

```
cor coroutine_2() -> GenMov[Gen=BigInt, Send=String] {
    let x = gen 1
    let y = gen 2
    let z = gen 3
    gen Arr::new(x, y, z).iter_mov().sum() 
}

fun function_2() -> Void {
    let coroutine = coroutine_2()
    coroutine.next("hello")
    coroutine.next("world")
    coroutine.next("!")
    let length = coroutine.next("ignored")
}
```

### Conflict with Memory Rules

<secondary-label ref="feature-frozen"/>

Owned objects being moved into coroutines poses no problems regarding the memory model, and ownership is simply
transferred. Borrows, however, present a more problem, as there can be no guarantee that the borrow is valid in the
entirety of the coroutine; the coroutine may be given a borrow, suspended, the borrow consumed in the caller context,
and then attempted to be used from the coroutine again.

To prevent this, [pinning](11-5-Pinning.md) is used; every value that is borrowed from into a coroutine must be pinned,
using the `pin` keyword. This tells the compiler that teh memory being borrowed from will not change location. This
prevents any moves or partial moves, ensuring the borrow remains valid. A pinned value being used inside a coroutine can
be unpinned at a suspension point. This will invalidate the coroutine, by marking it as "moved", and it will be unusable
from that point onwards.

### Coroutine Chaining

<secondary-label ref="doc-sect-wip"/>
<secondary-label ref="doc-sect-subj-update"/>
<secondary-label ref="examples-todo"/>

Coroutine chaining is a feature taken from Python, which allows for a coroutine to call another coroutine, and yield all
its values out in order. This is done by using the `gen with` expression, which just maps to a loop that yields all the
values from the inner coroutine. The inner coroutine must have a return type that is compatible with the outer
coroutine.

```
cor coroutine_3() -> GenMov[Gen=BigInt] {
    gen 1
    gen 2
    gen 3
}
```

The following generator:
```
cor coroutine_4() -> GenMov[Gen=BigInt] {
    gen with coroutine_3()
    gen 4
}
```

is converted to:
```
cor coroutine_4() -> GenMov[Gen=BigInt] {
    let inner = coroutine_3()
    loop case inner.next() then {
        is Some(value) => gen value
        is None => exit
    }
    gen 4
}
```

## Design Decisions
- **Different Generator Types**: There is no special type for immutable and mutable borrows, as the convention isn't
  tied to types, but rather to symbols. This means that there is no `Ref[T]` or `Mut[T]` that can be used to represent
  the convention of the generated values. As such, like with the three different function types representing their
  environments, three different generator types were created, to describe the convention of the expression to the
  compiler.

  This also increases the simplicity of type matching the expressions against the return type of the coroutine; rather
  than having to identifier a `gen` expression and ensure each matches it going forwards, each gen expression can be
  type-compared against the provided coroutine return type, and its generic arguments.

- **Two-way Data Passing**: Because a coroutine is resumed directly after suspension, it makes sense to tie the
  suspension and associated outgoing value, to the resuming and incoming data. As such, the `gen` keyword creates an
  _expression_, rather than the originally proposed _statement_; this allows it to be on the right-hand-side of a `let`
  statement, showing a clear link to the incoming data.

- **Using `GenMov` for `Gen=Void`**: Because the `Void` type is zero-sized, it is impossible to borrow it. Therefore,
  the `GenMov` type must be used when `Void` is the `Gen` generic argument.
