# 3.8. With Expressions

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## With Expression Definition

<secondary-label ref="feature-frozen"/>

- **With keyword**: The `with` keyword is used to introduce a context block over an object, which can have predefined
  `enter` and `leave` steps defined.

- **Expression alias**: A local variable followed by an `=` can optionally be used, to alias a result from the `enter`
  method. This might look like `with my_file = std::io::file::open("file.txt") { ... }`.

- **Expression**: An expression whose `enter` and `leave` methods are called. The `enter` method is called at the start
  of the block, and the `leave` method is called at the end of the block.

- **Inner scope**: The scope which contains the code that goes inside the block, surrounded in brace tokens. The local
  variable in the condition is injected into this scope.

## With Expression

S++ has **contextual blocks**, a feature taken from Python. A contextual block is a block of code that is executed under
a context manager, allowing an "enter" and "leave" method on an object to wrap the block of code. A type can be used as
a context manager if it superimposes either the `CtxRef` or `CtxMut` context manager type.

Context managers have several uses, the most common being cleaning resources up correctly, even if an error occurs. This
includes file management, socket management, mutex acquiring/releasing, etc.

If a context's `enter` method returns a value, then "aliasing" is allowed for the context block; this allows the value
returned from the `enter` method to be used in the block. This is useful for acquiring a resource and using it in the
block. The `leave` method always returns `Void`.

The `CtxRef` and `CtxMut` context manager types are used to specify whether the context manager is read-only or
read-write, respectively. There is no `CtxMov`, because both the `enter` and `leave` methods would have to consume
the `self` argument, violating the memory safety rules.

## Context Manager Definitions

```
cls CtxRef[Out=Void] {
    fun enter(&self) -> Self::Out { }
    fun leave(&self) -> Void { }
}

cls CtxMut[Out=Void] {
    fun enter(&self) -> Self::Out { }
    fun leave(&self) -> Void { }
}
```

## How to make a type a context manager

```
use std::CtxRef

cls Mutex[T] {
    value: T
}

sup [T] CtxRef[Out=T] on Mutex[T] {
    fun enter(&self) -> Self::Out {
        print("Locking mutex")
        ret self.value
    }

    fun leave(&self) -> Void {
        print("Unlocking mutex")
    }
}
```
