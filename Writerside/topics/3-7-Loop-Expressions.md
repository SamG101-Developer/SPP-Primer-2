# 3.7. Loop Expressions

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>


## Boolean Loop Expression Definition

<secondary-label ref="feature-frozen"/>

- **Loop keyword**: The `loop` keyword in used to introduce a conditional looping block. There are no other looping
  block keyword, and as such, this is the only way to introduce a loop.

- **Expression**: The boolean expression being evaluated against `true` per iteration.

- **Inner scope**: The scope which contains the code that goes inside the loop, surrounded in brace tokens. The local
  variable in the condition is injected into this scope.

- **Else block**: Optionally, an `else` block can be defined following the loop. This is only executed if there are 0
  iterations of the `loop` block, ie the condition was `false` on the first check.

### Boolean Example

```
loop i < 10 {
    std::print(i);
    i += 1;
}
```

## Iteration Loop Expression Definition

<secondary-label ref="feature-frozen"/>

- **Loop keyword**: The `loop` keyword in used to introduce a conditional looping block. There are no other looping
  block keyword, and as such, this is the only way to introduce a loop.

- **Local Variable**: A local variable definition, such as `mut i`. This will be the variable that is yielded to from
  the iterator coroutine. Its convention is fully dependent on the type of generator being yielded out of.

- **In keyword**: The `in` keyword separates the variable storing the yielded value from the iterator.

- **Iterator**: The iterator expression, which is a generator that yields a value to the local variable. It typically
  superimposes one of `IterMov`, `IterMut`, `IterRef`.

- **Inner scope**: The scope which contains the code that goes inside the loop, surrounded in brace tokens. The local
  variable in the condition is injected into this scope.

- **Else block**: Optionally, an `else` block can be defined following the loop. This is only executed if there are 0
  iterations of the `loop` block, ie the iterator was empty (returned before yielding).

### Iteration Example
```
loop i in vec.iter_ref() {
    std::print(i);
}
```

## Control Flow

There are 2 control flow statements that can be used within a loop block: `skip` and `exit`. These are used to skip the
current iteration, or exit the loop early, respectively, and are synonymous with `continue` and `break` in other
languages.

Nested loops can be exited from an inner loop, by stacking `exit` statements. This is done by using the `exit` statement
multiple times: `exit exit` would break out of 2 loops. Following an `exit` statement, either a `skip` statement can be
used: exit this loop then skip to the start of the outer loop; or a condition can be used, where the loop returns a
value to a variable, which would've been declared as `let x = loop condition { ... }`.

If a value is being assigned to from a `loop` expression, then every `exit` statement that would exit that loop, must
return the same type of variable, in the same way that every branch of a `case` expression would.

## Else Block

An `else` block can be provided, which is executed if either the loop condition was `false` before the loop begins its
first iteration (for a boolean expression), or if the range being iterated through is empty (ie the generator returns
before yielding).

**Without `while-else` block:**

:
```
case condition {
  loop condition {
    ...
  }
}
else { ... }
```

**With `while-else` block:**

:
```
loop condition {
  ...
}
else {
  ...
}
```
