# 5.4. Function Types

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

<secondary-label ref="doc-subj-update"/>

## 3 Function Types

In S++, all functions, methods and closures are first-class objects. They are either `FunMov`, `FunMut` or `FunRef`
types. Each type represents the way in which the function captures the environment - by moving in into the function,
borrowing it mutably, or borrowing it immutably.

Each function type contains generic arguments pertaining to the function's signature. For example, a function immutably
borrows its environment, takes a `Str` and returns a `Void` would have the signature `FunRef[Void, (Str)]`.

## `FunMov`

A method with a `self` or `mut self` parameter is a `FunMov` function, as it consumes its `self` environment. This means
that the object that owns this method will be consumed by the method, and non-accessible after the function has
returned. This is useful for functions that need to take ownership of the object, such as destructors.

## `FunMut`

A method with a `&mut self` parameter is a `FunMut` function, as it mutably borrows its `self` environment. This means
that the object that owns this method will be mutably borrowed, and can be used again after the function has returned.
This is useful for functions that need to modify the object that they belong to, for reasons such as updating internal
state.

## `FunRef`

A method with a `&self` parameter is a `FunRef` function, as it borrows its `self` environment. This means that the
object that owns this method will be borrowed, and can be used again after the function has returned. Free functions, ie
functions that do not have a `self` parameter, are also `FunRef` functions.
