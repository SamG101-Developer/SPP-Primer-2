# 11.1. Memory Model

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

## Memory Model

S++ uses "partially-automatic memory management". This means that the programmer never manually allocates or deallocates
memory. "Scope-Bound Resource Management", also known in C++ as "Resource Acquisition Is Initialization" (RAII), is used
to control object's lifetimes: memory allocation is done with object initialization, and memory de-allocation is done
with object destruction.

Borrows to variables must be manually created, using either the `&` or `&mut` tokens. This tells the compiler whether an
immutable or mutable borrow is being created.

To ensure that memory errors are always mitigated, 3 core techniques are used:

1. [Ownership Tracking](11-2-Ownership-Tracking.md) - this tracks the ownership of objects, and at any line of the
   program, the compiler can know if an object is holding an initialized object, or contains no object. In the case no
   object is contained, the symbol is unusable except for assignment (re-allocation).
2. [Second-Class Borrows](11-3-Second-Class-Borrows.md) - this is a way to borrow an object without moving it. This is
   useful for when a function needs to borrow an object, but the object must be used again after the function call.
   Borrows can only be taken at certain places, removing the need for lifetime analysis.
3. [The Law of Exclusivity](11-4-Law-of-Exclusivity.md) - this is a rule that states that only one mutable borrow xor
   any number of immutable borrows can exist at a time. This is enforced by the compiler, and if a mutable borrow is
   attempted while another borrow exists, the compiler will throw a compile time error.

The combination of these memory management techniques enforces that memory errors are always mitigated, and that no
additional syntax is required for any sort of lifetime analysis.

## Design Decisions

1. Immutable and mutable **borrows must be taken manually** with the `&` and `&mut` tokens. Whilst borrows could
   automatically be created at function call sites, and have the memory rules still apply, it would require knowing the
   function signature to know whether borrows were being created or not. Further to this, convention based overloading
   wouldn't be possible.

2. The use of **second-class borrows**. Using [second class borrows](11-3-Second-Class-Borrows.md) means that any form
   of lifetime analysis isn't required. This is because when a borrow is taken at a function call site (in a synchronous
   context), the borrow is guaranteed to be in a inner frame, and therefore automatically have a shorter lifetime than
   the owned object it's borrowing from.
