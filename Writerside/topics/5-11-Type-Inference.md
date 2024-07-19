# 5.11. Type Inference

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Type Inference

S++ uses a powerful 1-way type inference model, to ensure that a value's type is known at declaration site.

However, types in S++ must be "immediately inferable", which means that all the generic arguments of a type must be
inferable at the declaration site, and not set later with reverse inference.

Initialized vs non-initialized variable syntax is discussed in the [variable](4-1-Variables.md#defining-a-variable)
section. As seen with uninitialized variables, no value is provided, so a type annotation is required. There are a few
other places the same is true, with the full list below:

- Non-initialized variables
- Function parameters
- Function return types
- Class attributes

## Design Decisions

1. Types must be **immediately inferable**. This decision was made so that types can be fully inferred from the value's
   declaration, without needing to scan further code an unknown amount in order to determine the generics.
2. A function's parameter types **must be specified**. This is because function overloading is supported, and the exact
   types of the parameters must be provided in order for a match to take place. Whilst generics can be used in
   overloads, they integrate with the ambiguity checker, ensuring either the correct overload is chosen, or an error is
   raised.
3. A function's **return type must be specified**. Whilst the `ret` statements could all be analysed, this means that
   the entire function must be analysed in order to determine the return type. Requiring the return type in the
   signature mitigates this, and improves overall readability of the function and the program.
