# 3.6. Case Expressions

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Case Expressions

S++ doesn't have separate `if`, `match` and `?:` expressions; instead, there is a unifying conditional branching
expression: `case`. This improves the simplicity of the language and provides one way to do conditional branching. The
branching expression is named the `case` expression, and uses the `then` keyword to separate the condition from the
patterns.

Both the `is` and `==` operators are usable in the `case` expression. The `is` operator is used for pattern matching
against destructuring. The `==` cannot be used, because destructuring does not provide an entire object. Tuples can
sometimes use either, if an entire literal is provided with no bindings.

## Usages

<secondary-label ref="examples-todo"/>
<secondary-label ref="doc-sect-wip"/>
<secondary-label ref="doc-sect-subj-update"/>

The `case` expression is extremely flexible and can be used in many different ways. The following examples show the
different ways the `case` expression can be used.

### Basic Usage Example

The basic example shows the condition `person` being compared against the existing objects `john` and `jane`. If the
condition matches either of the objects, the corresponding block is executed. If the condition doesn't match any of the
objects, the `else` block is executed.

```
case person then
    == john { "hello john" }
    == jane { "hello jane" }
else { "hello stranger" }
```

### Different Condition Operators

All comparison operators, except for the `<=>` operator, can be used on branches. This
includes: `==`, `!=`, `<`, `>`, `<=`,`>=` and `is`. The `is` operator is used for pattern matching, and the `==`
operator is used for regular comparisons.

```
case age then
    <  0 { "error" }
    == 0 { "newborn" }
    <  18 { "child" }
    <  60 { "adult" }
else { "senior" }
```

### Pattern Matching (Object Destructure)

Pattern matching uses the `is` operator to destructure an object. The object is compared against the pattern, and if the
pattern matches, the block is executed. If the pattern doesn't match, the next branch is checked. The type following
the `is` keyword must match the expression type exactly.

```
case person then
    is Person(name="john", ..) { "hello john" }
    is Person(name="jane", ..) { "hello jane" }
else { "hello stranger" }
```

> All attributes must be present, unless the `..` token is used, which skips the rest of the attributes. This follows
> regular object destructuring rules.
> {style="note"}

> The `is` destructure also allows for variant types to be directly considered, combined with flow typing. This is
> shown in the below example.
> {style="note"}

### Pattern Matching (Object Destructure + Variant Types)

Because type-comparisons allow a composite type to be assigned to a variant type container, this also works for the type
comparisons of destructuring. That is, a variant type can be destructured by its composite types.

```
case optional_string then
    is Some(val) { "string is ${val}" }
    is None { "string is None" }
```

### Pattern Matching (Tuple Destructure)

Pattern matching can also be used to destructure tuples. There are cases where destructuring a tuple is the same as
checking for equality, if every item is a non-symbol value. In these cases, the `==` operator can be used. However, if
any item is a symbol, the `is` operator must be used.

```
case tuple then
    is (1, ..) { "tuple starts with 1" }
    is (2, ..) { "tuple starts with 2" }
else { "tuple is something else" }
```

> The `..` token can be used to skip the rest of the tuple. This follows regular tuple destructuring rules.
> {style="note"}

### Binding (Object Destructure)

Binding can destructure object and capture the values into the block. This allows for the values to be used in the
block. The variable names must match the attribute names exactly.

```
case person then
    is Person(name="john", age, ..) { "hello john, you are ${age}" }
    is Person(name="jane", age, ..) { "hello jane, you are ${age}" }
else { "hello stranger" }
```

> Both `name` and `age` are brought into their respective blocks, because both variable names appear, despite a
> comparison to the names too.
> {style="note"}

> The only variable names that can be used are the attribute names, because the class is being destructured. This
> follows regular object destructuring rules.
> {style="warning"}

### Binding (Tuple Destructure)

```
case tuple then
    is (a, 1) { "tuple is ($a, 1)" }
    is (b, 2) { "tuple is ($b, 2)" }
else { "tuple is something else" }
```

> Any variable names can be used, because the tuple is being destructured. This follows regular tuple destructuring
> rules.
> {style="note"}

### Multiple Patterns per Branch

```
case person then
    == john, jane { "hello john or jane" }
    == jack, jill { "hello jack or jill" }
else { "hello stranger" }
```

> Note the use of the `,` token rather than the traditional `|` token for multiple patterns. See the design decisions
> for more information.
> {style="warning"}

> This is only allowed for non-`is` destructures, so that only 1 set of variables are considered. This might expand into
> allowing multiple patterns as long as the same variables are introduced by each pattern.
> {style="warning"}

### Nested destructuring

### Pattern Guards

### Unrelated Boolean Conditions

Using the `else` keyword with a new `case` expression follows the typical `else if` found in C-like languages. This
allows for unrelated conditions to be checked in the same block. This requires a separate parsing rule to remove the
extra `{}` tokens following the `else` keyword.

```
case some_condition().attribute then
   == 1 { "condition is 1" }
else case other_condition().attribute then
   == 2 { "condition is 2" }
   == 3 { "condition is 3" }
else {
   "condition is something else"
}
```

### Ternary Operator Mocking

## Design Decisions

1. An **operator is required for every branch**. It was considered to add an optional operator to the end of the
   condition, which would then apply to every branch. However, whilst improving conciseness by a very small amount, it
   offered two ways to do the same thing if the operators are all the same. This did not outweigh the minor reduction in
   characters required. As such, it is not an included feature. Every branch requires an operator.
