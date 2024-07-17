# 2.4. Comments

<primary-label ref="header-label"/>

<secondary-label ref="doc-complete"/>

## Introduction to Comments

Comments in S++ take inspiration from Python, rather than the traditional C-family of comments.

## Types of Comments

There are 2 types of comments in S++: single-line comments and multi-line comments. Single-line comments are denoted by
a `#` character, while multi-line comments are wrapped by `##` characters.

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

Multi-line comments can be embedded within normal code too. These are sometimes referred to as "inline-comments".

```
func(param1: ## important ## : std::Str) -> std::Void { }
```
