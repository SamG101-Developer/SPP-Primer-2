# 9.4. Generic Compilation

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Generic Compilation

A lot of languages that use templates or generics will compile the generic code for each type that is used. However, S++
doesn't do this to keep compile times low. Instead, S++ creates a new scope for the type, say `Vec[Str]`, and only
stores the generic substitution for the type in that scope. All attributes or function code is pulled from the base
scope - `Vec[T]` - and ran through the substitution.
