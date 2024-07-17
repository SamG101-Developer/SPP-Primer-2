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

#### Owned Object

An owned object is a non-borrowed value, whose lifetimes is, at present, limited to the current scope. It will have
either been created in the current scope, or passed into the current scope as an argument or a coroutine yield. An owned
object's lifetime can be extended by moving into an object initializer, thereby transferring ownership of the value from
the current scope (function) to the object being initialized.

#### Borrowed Object

A borrowed object is a non-nullable pointer to an owned-object, denoted by the `&` token. An immutably borrowed object
is also known as a "reader" and a mutably borrowed object is known as a "writer." A borrow is always valid, and can
never outlive the lifetime of the owned object that is being borrowed. Unlike C++, the default mutability of a borrowed
object is immutable.

#### Move

A move is the transfer of ownership of an object from one scope to another. This is done by passing the object as an
assignment value, function argument, object initialization argument, or a coroutine yield. The owned object now "
non-initialized", and is therefore no longer valid in the current scope, and the variable cannot be used until it is
reinitialised.

#### Partial-Move

A partial-move is the transfer of ownership of part of an object, such as moving the value of an attribute off of an
object. This is done in the same way as a standard move, but as part of a larger object. The object whose attribute has
been moved from is now "partially-moved", and therefore is "partially-initialised".

#### Fully-Initialised Object

An object is fully initialized when all of its fields have been assigned a value. All borrowed objects are fully
initialized by default, as moving out of a borrowed context is prohibited, and taking a borrow of a partially
initialized object is also not allowed. This ensures the validity of a borrowed object.

#### Definition vs Declaration

In languages like C++, forward declarations are required. S++ uses a multi-stage compilation module, so definition order
does not matter. As such, forward declarations, and any other type of declarations are not required, and have no syntax
that would allow for it. Therefore, all operations which introduce a symbol will be referred to as definitions.
