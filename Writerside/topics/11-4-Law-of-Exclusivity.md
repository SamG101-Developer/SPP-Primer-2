# 11.4. Law of Exclusivity

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

## The Law of Exclusivity

The Law of Exclusivity prevents overlapping mutable borrows to the same owned object, which can lead to data races and
inconsistent memory reads. In short, 1 mutable borrow xor any number of immutable borrows can exist at one time. The Law
of Exclusivity is enforced by the compiler at compile time.

There laws, in detail, are as follows:

1. No more than 1 overlapping mutable borrow can be made at any given time.
2. Any number of overlapping immutable borrows can be made at any given time.
3. Law 1 xor Law 2 must be followed at all times.

Fundamentally, whilst a mutable borrow exists to some memory location, no part of that memory location, however small,
can be borrowed in any way. If the existing borrow is immutable, however, than any number of immutable borrows to any
part of that section of memory can be borrowed immutable.

### Overlapping Borrows

These laws apply to **overlapping** borrows. An overlapping borrow is where one borrow inclusively contains another
borrow. That is, the memory location that the second borrow points to, is within the memory location that the first
borrow points to, or vice-versa.

For example, `x.a` overlaps `x`, because `x` contains `x.a`. Therefore, if `x.a` is borrowed mutably, then `x` cannot
be borrowed in any way, and vice-versa. However, `x.a` and `x.b` do not overlap, because they are separate memory
locations and do not contain each other. As such, `x.a` and `x.b` can be borrowed in any way simultaneously.

| Memory Region (X) | Memory Region of Attributes | Part  |
|-------------------|-----------------------------|-------|
|                   | `1000 -> 1008`              | `x`   |
|                   | `1000 -> 1004`              | `x.a` |
|                   | `1004 -> 1008`              | `x.b` |
