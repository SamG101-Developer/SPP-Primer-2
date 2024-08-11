# 6. Modules

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

**Modules**

1. Module Structure
2. Module System
3. 3rd Party Modules

`TODO`

:
The `mod` keyword defines a `.spp` file as a [module](#) to be included in the [module tree](#) for compilation. The
name of the module must follow the `mod` keyword, and must reflect the directory that the file is in, up-to and
including the `src`folder. It must be the first line in the file.

:
The file `src/foo/bar/baz.spp` requires the definition `mod src::foo::bar::baz`. If a file is being written but isn't
completed, commenting out the `mod` definition will exclude it from the module tree.
