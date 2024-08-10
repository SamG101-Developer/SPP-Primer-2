# 8.4. Function Recursion

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Function Recursion

<secondary-label ref="doc-sect-wip"/>

<secondary-label ref="doc-sect-subj-update"/>

<secondary-label ref="feature-not-impl-yet"/>

S++ supports function recursion, and re-writes every recursive function into a tail-recursive form. This means that
every recursive function can utilize tail-call optimization, and can be optimized by the compiler to run in constant
stack space.

The re-write method (AST manipulation) follows the steps:
- **TODO**
