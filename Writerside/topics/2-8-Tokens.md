# 2.8. Tokens

## Tokens

- `{`
- `}`
- `[`
- `]`
- `(`
- `)`
- `:`
- `_`
- `..`
- `?`
- `.`
- `->`
- `@`
- `+`
- `-`
- `*`
- `/`
- `%`
- `**`
- `%%`
- `&`
- `|`
- `<=>`
- `==`
- `!=`
- `>`
- `<`
- `>=`
- `<=`
- `=`
- `+=`
- `-=`
- `*=`
- `/=`
- `%=`
- `**=`
- `%%=`
- `??`
- `,`

{columns="4"}

## Token Descriptions

### Bracket Tokens

`{ }`
: - Introduce a new scope (`cls/sup/fun/case/loop/with/else` or unnamed).

`[ ]`
: - Group generic parameters or arguments.
: - Group long-hand generic constraints (`where`)
: - Group lambda captures.
: > Lambda capture syntax is subject to change. {style="warning"}

`( )`
: - Group function parameters or arguments.
: - Group object initialization arguments.
: - Group destructure operation values.
: - Group multiple type imports.
: - Parenthesized expressions.
: - Tuple literals.
: > Type imports might switch to `{}` depending on updated scoping rules. {style="warning"}
