# 5.14. Type Comparisons

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Type Comparisons

S++ doesn't fully qualify types in the symbol table, or even substitute generics. All type-comparisons are done by
symbolic equality with double-individual scoping options. What this means, is two types can be compared in separate
scopes, and the resulting symbol of each scope inspection is then compared. This reduces complexity in the compiler.
