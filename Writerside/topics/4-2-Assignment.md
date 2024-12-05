# 4.2. Assignment

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Assignment

S++ uses the `=` operator to assign a value to a variable. There are
also [compound assignment operators](2-8-Tokens.md#compound-assignment-operators), which combine an assignment with a
binary operation.

An assignment requires a variable or attribute on the left-hand side, and an expression on the right-hand side. The type
of the variable and the value must match exactly, and the memory status tied to the symbols must match too; if the
left-hand-side variable is a borrow, then the value being assigned to it must also be a borrow.

As an assignment mutates the variable being assigned to it, the variable must be mutable. If the variable is not
mutable, then a compile-time mutability error will be thrown. A non-mutable variable holding an `&mut` symbol can be
mutated, but not assigned too. A mutable `&` variable can be assigned to, but not mutated.

## Compound Assignment

Compound assignment operators are used to combine an assignment with a binary operation.

### Compound Assignment Operators {collapsible="true"}

- `+=`
- `-=`
- `*=`
- `/=`
- `%=`
- `**=`
- `%%=`
- `??=`

{columns="2"}

### Transformation

Each of these operators, like regular binary operators, are converted into function calls, except for the `??=`operator,
which has a separate transformation rule detailed below. There is a constraint, as with all assignment, that the lhs is
a variable / attribute. Thus, assignment cannot be stacked, for example `1 += 2 *= 3` is not allowed. This is the same
as C++. The following table shows the transformation from compound assignment to function call:


<table>
<tr>
<th>From</th>
<th>To</th>
</tr>

<tr>
<td>

```
let mut x = 1
x += 5
```
</td>
<td>

```
let mut x = 1
x.add_assign(5)
```
</td>
</tr>

<tr>
<td>

```
let mut x = func_that_returns_opt()
x ??= 5
```
</td>
<td>

```
let mut x = func_that_returns_opt()
x = case x of
    is type(x)::Pass { x }
    is type(x)::Fail { 5 }
```
</td>
</tr>
</table>


> The `??=` operator transformation will probably change to a utility method superimposed over the `Opt[T]` type, as
> there will be a method for settings the value of an `Opt[T]` type to a default value if it is `None`.
> {style="warning"}
