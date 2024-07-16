# 2.6. Keywords

S++ uses a small keyword set of 29 keywords. The keywords are not context dependant, and can therefore not be used as
identifiers in non-keyword contexts. This makes the language easier to learn and understand, and to make the code more
readable.

Keywords in S++ are not commonly seen in other languages, but convey the same meaning as their counterparts in other
languages. For example, `loop` is used to convey a standard `while` or `for` loop. Keywords of the same group have the
same length, to make the code more aligned.

The following table lists all the keywords in S++:

|             |      |      |       |       |
|-------------|------|------|-------|-------|
| [mod](#mod) | cls  | sup  | fun   | use   |
| let         | mut  | case | else  | loop  |
| with        | skip | exit | ret   | gen   |
| where       | is   | as   | true  | false |
| self        | Self | and  | or    | not   |
| on          | in   | then | async |       |

## Module Level Keywords

#### `mod`

The `mod` keyword defines a `.spp` file as a [module]() to be included in the [module tree]() for compilation. The name
of the module must follow the `mod` keyword, and must reflect the directory that the file is in, up-to and including
the `src`folder. It must be the first line in the file.

The file `src/foo/bar/baz.spp` requires the definition `mod src::foo::bar::baz`. If a file is being written but isn't
completed, commenting out the `mod` definition will exclude it from the module tree.

#### `cls`

The `cls` keyword is used to define a [class](). Generic arguments can be optionally provided, and the class name must
follow. Only [state]() can be defined in a `cls` block.

Classes can only be defined at the module level. The order of class definition doesn't matter, because of
the[multi-stage]() compilation model.

#### `sup`
