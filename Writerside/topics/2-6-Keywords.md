# 2.6. Keywords

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

<secondary-label ref="doc-subj-update"/>

## Introduction to Keywords

S++ uses a small keyword set of 29 keywords. The keywords are not context dependant, and can therefore not be used as
identifiers in non-keyword contexts. This makes the language easier to learn and understand, and to make the code more
readable.

Keywords in S++ are not commonly seen in other languages, but convey the same meaning as their counterparts in other
languages. For example, `loop` is used to convey a standard `while` or `for` loop. Keywords of the same group have the
same length, to make the code more aligned.

## Keywords

- `mod`
- `cls`
- `sup`
- `fun`
- `use`
- `let`
- `mut`
- `case`
- `else`
- `loop`
- `with`
- `skip`
- `exit`
- `ret`
- `gen`
- `where`
- `is`
- `as`
- `true`
- `false`
- `self`
- `Self`
- `and`
- `or`
- `not`
- `on`
- `in`
- `then`
- `async`

{columns="4"}

## Keyword Descriptions

### Module Level Keywords {collapsible="true"}

`mod`
: - Define a module to be included in the module tree for compilation.

`cls`
: - Define a class (state only: attributes).

`sup`
: - Define a regular superimposition block (behaviour: methods, typedefs)
: - Define an inheritance superimposition block
: - Pass superclass instances to object initializations.
: `on`
    : - Superimpose a class "on" another class.

`fun`
: - Define a subroutine function.
: - Define a subroutine lambda functions.

`cor`
: - Define a coroutine function.
: - Define a coroutine lambda functions.

`use`
: - Define a type alias.
: - Define a type reduction.
: `as`
    : - Alias a type as part of type aliasing.

### Variable Keywords {collapsible="true"}

`let`
: - Define a variable.
: `mut`
    : - Mark a variable as mutable.

`mut`
: - Mark a type's borrow convention as mutable.
: - Take a mutable borrow of a variable.

### Control Flow Keywords {collapsible="true"}

`case`
: - Define a conditional branching block.
: `then`
    : - Define a block of code to be executed after a conditional block.
: `else`
    : - Define a "default" branch for if no other branches match.

`loop`
: - Define a conditional looping block.
: - Define an iteration-/range-based looping block.
: `in`
    : - Define a range-based loop.
: `else`
    : - Define a "default" branch for a `loop` block.

`with`
: - Define a context block.

`skip`
: - Skip the current iteration of a loop.

`exit`
: - Exit the current loop.

`ret`
: - Return a value from a function.

`gen`
: - Yield a value from a coroutine.
: `with`
    : - Yield every element of a generator.

### Type Keywords {collapsible="true"}

`where`
: - Define a block of type constraints.

`is`
: - Check if a variant is of a certain type.

`true`
: - Boolean value `true`.

`false`
: - Boolean value `false`.

`self`
: - Reference to the current instance of a class.

`Self`
: - Reference to the current type of a class.

### Logical Keywords {collapsible="true"}

`and`
: - Logical `and` operator.

`or`
: - Logical `or` operator.

`not`
: - Logical `not` operator.

### Other Keywords {collapsible="true"}

`async`
: - Define an asynchronous function call.

`else`
: - Define a "default" value for object initializations.
