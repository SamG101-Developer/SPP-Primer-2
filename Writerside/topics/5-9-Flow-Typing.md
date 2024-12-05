# 5.9. Flow Typing

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Flow Typing

Flow typing is used to allow variant types to be decomposed into their constituent types. This is done by using the `is`
keyword in a `case` pattern branch. When the `is` keyword is used, the type of the variable being inspected is treated
as the type being checked against. This is called flow typing, and is used to reduce the amount of casting that is
required in the code.

When `case` blocks are used for assignment, exhaustive branching is required. This either requires an `else` block, or
every single constituent type of the variant to be checked for. If an `else` block is used, then the type of the
variable is treated as a variant of all the types that were not checked for.

```
let val = function_returning_option()  # Returns an Opt[Bool]
case val of
    is Some(val) { std::print("Value present", val) }
    is None() { std::print("No value") }
    
let val = function_returning_result()  # Returns a Ret[Str, Err]
case val of
    is Pass(msg) { std::print("Pass", msg) }
    is Fail(err) { std::print("Fail", err) }
```
