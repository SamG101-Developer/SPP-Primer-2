# 1.1. Terms &amp; Definitions

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

There are a number of terms that are used throughout the specification that may not be immediately clear. This section
aims to define these terms.

#### Arguments & Parameters

An argument is given at the call site of the target function, is passed into the function, and received as a parameter.
The argument is the value that is passed to the function, and the parameter is the variable that the argument is
assigned to. This also applies to generic arguments and parameters.

#### Variables & Values

A **variable** is named memory location in a program that can store a value. It serves as a reference point for
accessing, modifying, or performing operations on the data it holds. Variables are defined by their type, which dictates
to the compiler how they are managed in memory.

A **value** is the data that is stored in a variable. Values can represent entities such as numbers, booleans, strings,
or more complex object structures. Values can be moved, copied and borrowed. A variable can be _borrowed from_, where it
lends its value to another variable, but retains ownership of the value.

#### Owned Object

An **owned object** is a value that resides in a memory location explicitly associated with a variable, where the
variable holds full control and responsibility for the object's lifecycle. Unlike borrowed objects, an owned object is
not shared, or referenced by other variables upon introduction into the current scope; it's memory is uniquely tied to
the owning variable.

#### Borrowed Object

A **borrowed object** is a value accessed through a guaranteed valid reference, allowing temporary usage of memory
managed by another variable. Borrowing enables access to a value without transferring ownership, facilitating shared or
exclusive usage under controlled conditions. Borrowed objects will always be valid, and cannot outlive the lifetime of
the owned object that is being borrowed.

#### Move

The **move** operation is a mechanism by which ownership of a value is transferred from one variable to another. After
the move, the original variable no longer retains access to the value, and any attempt to use the variable results in a
compile-time memory error. Typically, a move operation will occur when passing a value into a function, returning from a
function, or assigning a value to a variable.

#### Partial-Move

A **partial move** is an operation where ownership of a subset of an owned object is transferred to another variable,
such as the field of an object. The remaining object is now in a partially-moved state, and is considered partially
initialized. Partially initialized objects conform to strict memory rules, similar to that of non-initialized objects.
An owned object with no partial moves is fully-initialized.

#### Definition vs Declaration

A **declaration** introduces the name of a program entity, without specifying details, such as function without a body.
A **definition** typically introduces the detail to a symbol. In S++, declarations are not a recognized concept, as the
multi-stage nature of the compiler allows for symbols to be defined in any order. Therefore, all operations which
introduce a symbol will be referred to as definitions, and all definitions fully specify the symbol.
