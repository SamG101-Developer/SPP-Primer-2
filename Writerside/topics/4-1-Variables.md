# 4.1. Variables

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Defining a Variable

The `let` statement is used to define a variable, introducing a new symbol into the current scope's symbol table.
The `let` statement can introduce an initialized or uninitialized variable. If the variable is non-initialized, then is
must be given a type annotation, as there is no value to infer from. As an initialized variable's value can always be
fully type-inferred, provided a type annotation is not syntactically allowed, as it introduces redundancy and extra
noise.

## Non-initialized Variables

A non-initialized variable can be defined, but cannot be used until it has been assigned a value. The [memory model]()
uses [ownership tracking]() to monitor the owner of the variable, and will not allow the variable to be used until it
is initialized.

If a variable is non-initialized, and only initialized in some branches of a conditional block, then a "potentially
uninitialized" error will be thrown - a variable must either be initialized or non-initialized at the end of every
branch of a conditional block.

The following syntax is used to define a non-initialized variable:

```
let variable_name: Type
```

## Initialized Variables

An initialized variable contains a left-hand-side part, and a value part. The left-hand-side can be one of:

- Single variable name
- Tuple destructure
- Object destructure
- Combination of the above with nesting

A type annotation cannot be provided for initialized variables, because the type of the value would have to be inferred
to check it matches the provided type annotation anyway, so it is redundant. Therefore, type annotations are not allowed
for initialized variables, and full type inference is used.

### Single Variables

Variables can be defined with a single identifier - this is the most common way to define a variable. The syntax is:

```
let variable_name = value
```

### Tuple Destructure

Variables can be defined by destructuring a tuple. The syntax is:

```
let (variable_name_0, variable_name_1) = tuple_value_0
```

Underscores can be used to skip values in the tuple. The syntax is:

```
let (variable_name_0, _, _, variable_name_3) = tuple_value_1
```

Ellipsis can be used to skip 1 group of values in the tuple. The syntax is:

```
let (variable_name_0, .., variable_name_1) = tuple_value_2
```

Ellipsis can be used to bind multiple values in the tuple to a single variable. The syntax is:

```
let (variable_name_0, ..variable_name_1) = tuple_value_3
```

### Object destructure

Variables can be defined by destructuring an object. The syntax is:

```
let Point(x, y, z) = point_value
```

Ellipsis can be used to skip the rest of the object's attributes. The syntax is:

```
let Point(x, y, ..) = point_value
```

### Nested Destructure

Variables can be defined by destructuring a nested structure. The syntax is:

```
let Vec(point=Point(x, ..), ..) = vec_value
```
