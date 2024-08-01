# 11.3. Second Class Borrows

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

## Second Class Borrows

S++ uses second class borrows. These are identical in functionality to Rust's borrows, but are not first class types.
This means that the borrow status isn't stored with the type, but with the variable's symbol directly. There are a lot
of restrictions as to where a borrow can be taken. This eliminates the need for lifetime annotations, as the borrow
checker can infer the lifetime of a borrow from the code itself, by scoping rules.

Borrows can only be taken at two places:

1. Function call sites: `func(&var1, &mut var2)`
2. Yielding from a coroutine: `gen &var1`

### Function Call Sites

Taking a borrow at a function call site is guaranteed to be safe, because the lifetime of the borrow will always be less
than the lifetime of the owned object. This is because the owned object must exist in the current scope, and the borrow
is passed into the function call (a new inner scope). Scopes represent frames on the stack, meaning that the lifetime of
inner scopes must be less than the lifetime of the current scope.

An additional bonus to this model is that borrows themselves can be stored on the stack, as the borrow cannot escape the
frame that it's created in at the function call site. They are automatically dropped when the frame is popped off the
stack, meaning that borrows are automatically released.

### Yielding from a Coroutine

Taking a borrow at a `gen` expression (coroutine yield) is guaranteed to be safe, because control will always return to
the coroutine when it is resumed again. This means that the owned object (which is in the coroutine scope) can never be
used until the coroutine is resumed, and the borrow is released.

This is a critical feature for iteration, which is solely based on coroutines. For example, the `iter_ref`
and `iter_mut` method both yield borrows with the `GenRef[T]` and `GenMut[T]` types respectively.

## Lifetime Analysis

The use of second class borrows means that lifetime analysis becomes trivial. All function call site borrows move into
an inner frame, meaning that they can never outlive their owned object. Yielding borrows can never outlive their owned
objects, because control cannot be returned to the coroutine (where the owned object exists), until the coroutine is
resumed and the borrow is therefore released.

## Stacking Borrows

Borrows can be stacked, by using the `&` operator on a variable that is already borrowed, but are automatically reduced
to 1-layer borrows. This means that a borrow of a borrow can be taken, increasing the number of active borrows, but both
are direct borrows of the corresponding owned object. That is, both borrows are `&T` types, not `&T` and `&&T` types.
This differs from `C++`, where stacking pointers for example can create `T**` types.

## Mutable Borrows vs Mutable Variables

The mutability of a variable and its borrow can be different. The difference revolves around the mutability of the
symbol, ie can the value inside it be replaced, and the mutability of the value itself, ie can the existing value be
mutated:

| Borrow? | Example                        | Value Mutability           | Borrow Mutability      |
|---------|--------------------------------|----------------------------|------------------------|
| `N`     | `fun f(x: T) -> Void`          | `x` can not be re-assigned | `x` can not be mutated |
| `N`     | `fun f(mut x: T) -> Void`      | `x` can be re-assigned     | `x` can be mutated     |
| `Y`     | `fun f(x: &T) -> Void`         | `x` can not be re-assigned | `x` can not be mutated |
| `Y`     | `fun f(mut x: &T) -> Void`     | `x` can be re-assigned     | `x` can not be mutated |
| `Y`     | `fun f(x: &mut T) -> Void`     | `x` can not be re-assigned | `x` can be mutated     |
| `Y`     | `fun f(mut x: &mut T) -> Void` | `x` can be re-assigned     | `x` can be mutated     |
