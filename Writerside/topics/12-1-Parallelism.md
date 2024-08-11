# 12.1. Parallelism

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Parallelism

S++ uses threading primitives to achieve parallelism. The `Thread` class, and its associated primitives, take
inspiration from the C++ threading system, but with a more "Pythonic" API. There are no special keywords, like
Java's `synchronized`, because mutexes are used to lock specific data structures. Mutexes are commonly combined with
the `with` context block keyword.

## Mutexes

<secondary-label ref="feature-frozen"/>
<secondary-label ref="doc-sect-wip"/>

**Mutex Locking Example**

:
Mutexes can be used to wrap data structures, and the internal value is accessibly only when the mutex is locked:

:
```
let mutex = {
    let internal_value = 0
    Mutex(value=internal_value)
}
with internal_value = mutex.lock_mut() {
    internal_value += 1
}
```

:
The context manager releases the lock when the block is exited, ensuring that the data structure is not accessed
concurrently. This can be seen in the `std::Mutex::exit_mut` method, which is superimposed via the `std::CtxMut` type.

### Sharing Data between Threads

<secondary-label ref="feature-frozen"/>
<secondary-label ref="examples-todo"/>

Sharing data between threads requires the `Shared` reference-counted type to be used in combination with a `Mutex`.

<deflist>
<def title="Sharing Example 1">

> Note that this example will fail, because of the double move of the `m` variable, into both the closures `f1` and`f2`.
> {style="warning"}

```
let m = Mutex(value=0)
let h = Vec[Thread]()
```

```
let f1 = fun () [m] -> Void {
    with value = m.lock_mut() { value += 1 }
}
let f2 = fun () [m] -> Void {
    with value = m.lock_mut() { value += 1 }
}
```

```
h.emplace_tail(Thread(target=f1))
h.emplace_tail(Thread(target=f2))
h.iter_mut().for_each(Thread::start)
```
</def>

<def title="Sharing Example 2">

> This example will work, because the `Shared[T]` type is used to wrap the mutex. This allows the mutex to be shared
> between threads, without a double move.
> {style="note"}

```
let m = Shared(value=Mutex(value=0))
let h = Vec[Thread]()
```

```
let f1 = fun () [m1 = m.clone()] -> Void {
    with value = m1.lock_mut() { value += 1 }
}
let f2 = fun () [m1 = m.clone()] -> Void {
    with value = m1.lock_mut() { value += 1 }
}
```

```
h.emplace_tail(Thread(target=f1))
h.emplace_tail(Thread(target=f2))
h.iter_mut().for_each(Thread::start)
```
</def>
</deflist>
