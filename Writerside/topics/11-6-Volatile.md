# 11.6. Volatile

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Volatile

A volatile value is a value that can be changed by something outside the current program. This is useful for interfacing
with hardware or other systems that can change values without the program's knowledge. It will stop the compiler
optimizing the value away, as it knows that the value can change at any time.

There is no keyword for volatile values, because keywords are strictly used for introducing blocks or other language
constructs. It also can't be a decorator, as these apply to classes and functions.

Instead, the `std::MemChunk` object is used to get the object at a memory address, and cast into the correct type. The
main issue is this breaks safety guarantees, because the memory being accessed might be being used in the current
program elsewhere.

To get around this, the `MemChunk` type can only be used with addresses that are specifically known to be for hardware.
A complete list of these addresses are:

```
let port: MemChunk = MemChunk[U64](from=0x1234);
```
