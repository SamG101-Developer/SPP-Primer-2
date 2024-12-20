# 2.4. Comments

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

## Types of Comments

Single-line, multi-line, and inline comments are supported in S++. The `#` character is used for single-line comments,
which terminate with a newline. Multi-line comments are wrapped in `##` characters, and can span multiple lines. Inline
comments are syntactically identical to multi-line comments.

## Syntax

### Single-line Comments

Single-line comments are denoted with a single `#` character, followed by the comment text. They can be placed at the
end of a line of code, or on a line by themselves. Any Unicode character can be used in a single-line comment.

```
# Variable declaration

let x = 5  # This is a single-line comment
```

### Multi-line Comments

Multi-line comments are denoted by `##` characters at the start and end of the comment. They can span multiple lines.
Any Unicode character can be used in a multi-line comment.

```
##
This is a multi-line comment
This can span multiple lines
##
```

### Inline Comments

Inline comments are syntactically identical to multi-line comments, but are placed within a line of code.

```
func(param1: ## important ## : std::Str) -> std::Void { }
```
