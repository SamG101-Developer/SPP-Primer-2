# 2.3. Whitespace &amp; Line Termination

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## White Space

S++ uses spaces (ASCII 32) exclusively for whitespace. Using any other space-like character, such as the tab character
(ASCII 9), will result in a compile time lexical error. This enforces uniformity in code formatting across all S++
programs.

Whitespace is always skipped in the parser, making it irrelevant to the parsing of any code, as seen in languages like
Rust and C++. This means that S++ is a **whitespace-insensitive language**.

Whilst whitespace is ignored by the parser, it is still important to use whitespace to make code more readable. See
the [style guide](15-Style-Guide.md) for more information recommendations on whitespace usage.

## Line Termination

S++ moves away from the C-family of languages that use the `;` token to terminate lines. Instead, the `\n` newline
character is used, to provide a more readable and simpler syntax.
