# 1.1. Terms &amp; Definitions

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

<secondary-label ref="doc-subj-update"/>

There are a number of terms that are used throughout the specification that may not be immediately clear. This section
aims to define these terms.

#### Arguments & Parameters

An argument is given at the call site of the target function, and is passed to the function as a parameter. The argument
is the value that is passed to the function, and the parameter is the variable that the argument is assigned to. This
also applies to generic type arguments and parameters.

#### Variables & Values

A variable is a named memory location. A value is the data that is stored in the memory location. A value can be moved
between variables. Not every variable has a value.

#### Owned Object

An owned object is a value that is stored in a variable. The term "owned" refers to the fact that the value is being
referred to, and not a borrowed instance of it. This means that the variable containing the value is responsible for the
value's lifetime; the variable going out of scope causes de-allocation of the value.

#### Borrowed Object

A borrowed object is a non-nullable pointer to an owned-object (a value stored in a variable). A borrowed object is
always valid, and cannot outlive the lifetime of the owned object that is being borrowed. This is enforced by the rules
of the memory model. A borrowed object can be either immutable or mutable, defaulting to immutable. An immutably
borrowed object is known as a "reader" and a mutably borrowed object is known as a "writer." Taking a borrow of a borrow
doesn't stack the borrow, ie doesn't create a `&&` type, but takes a borrow of the owned object under the original
borrow.

#### Move

A variable "owns" a value. As such, values can be moved between variables, transferring their ownership. For
example, `let x = y` moves the value from `y` into `x`. The variable `y` is now non-initialized. A value's ownership can
be transferred by passing a variable as an assignment value, function argument, object initialization argument, or a
coroutine yield. This moves the value inside the variable into the destination variable, in a different memory location.
The original variable is now non-initialized, and cannot be used until it has been re-initialized with a new value.

#### Partial-Move

A partial-move is the transfer of ownership of part of an object, such as moving the value of an attribute off of an
object. This is done in the same way as a standard move, but as part of a larger object. The object whose attribute has
been moved from is now "partially-moved", and therefore is "partially-initialized".

#### Fully-Initialized Object

An object is fully initialized when all of its fields have been assigned a value. All borrowed objects are fully
initialized by default, as moving out of a borrowed context is prohibited, and taking a borrow of a partially
initialized object is also not allowed. This ensures the validity of a borrowed object.

#### Definition vs Declaration

In languages like C++, forward declarations are required. S++ uses a multi-stage compilation module, so definition order
does not matter. As such, forward declarations, and any other type of declarations are not required, and have no syntax
that would allow for it. Therefore, all operations which introduce a symbol will be referred to as definitions.
