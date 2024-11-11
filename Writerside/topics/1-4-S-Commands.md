# 1.4. S++ Commands

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## S++ Commands

S++ has a small set of commands that are used to set up, configure, and run a project. These commands are used to create
new projects, build projects, run projects, and more. The commands are:

- `s++ init`: Creates a new S++ project with the given name. The project structure is created in the current folder. The
  project structure is detailed in the [Project Structure](1-3-Project-Structure.md)
- `s++ build`: Builds the current project. The compiler will compile all `.spp` files in the `src` folder, and output
  the binary to the `bin` folder. All `vcs/ext` dependencies will be linked automatically.
- `s++ run`: Builds and runs the current project. The steps are the same as `s++ build`, but the binary is executed
  immediately after compilation.
- `s++ clean`: Cleans the project. The `bin` folder is emptied, and the project is ready to be built again. The `bin`
  folder is not deleted.
- `s++ update`: Updates the vcs dependencies of the project. The compiler will check the `vcs` folder for newer versions
  of the dependencies, and will offer to update them individually, or all at once.
- `s++ help`: Displays the help message for the S++ compiler. This message contains a list of all commands, and a brief
  description of each command.
- `s++ version`: Displays the version of the S++ compiler that is currently installed. This is useful for debugging
  purposes, and for checking if the compiler is up to date.
- `s++ test`: Runs the tests in the `tst` folder. **TODO**.

### `spp init`

The `init` command is used to create a new S++ project. It creates every required folder and file in the project
structure inside the current directory, and will fail if the current directory is not empty. There are no arguments
passable to the `init` command, as the `spp.toml` configuration file that is generated will contain all the metadata
that can be updated. See the [Project Structure](1-3-Project-Structure.md) topic for more information on the project

### `spp build`

The `build` command is used to compile the current project. It will recursively search teh `src` folder for all `.spp`
files. The `vcs` and `ext` folders are then compiled. Finally, the stub files inside the `ffi` folder are compiled. The
compiler will then link the `dlls` associated with the stub files, and output the binary, and dependency dlls, to the
`bin` folder, inside a subfolder of the build type (debug or release).

If the project is already built and only some files have been modified, then only those files are re-compiled. Because
of the module system, only the files that are directly modified will be re-compiled - not even the files that import the
modified files will be re-compiled. Standard file hashing is used to determine if a file has been modified.

Because order of file compilation doesn't matter, the compilation can be parallelized. The compiler will heavily thread
the compilation process to speed up the build time. Only the codegen step of the compiler is parallelized, as the
analysis stages modify the symbol tables.

| Build mode flag group | Description                                                                                    |
|-----------------------|------------------------------------------------------------------------------------------------|
| `--debug`             | Compiles the project in debug mode. This will output the binary to the `bin/debug` folder.     |
| `--release`           | Compiles the project in release mode. This will output the binary to the `bin/release` folder. |
| `--clean`             | Cleans the project before building. The `bin` folder is emptied, and the project is re-built.  |
| `--executable`        | Compiles the project as an executable. This is the default.                                    |
| `--shared`            | Compiles the project as a shared library.                                                      |
| `--static`            | Compiles the project as a static library.                                                      |

The default command is `spp build --debug --executable`.

### `spp run`

The `run` command is used to compile and run the current project. The steps are the same as the `build` command, but the
binary is executed immediately after compilation. The command is equivalent to running `spp build` followed by
`./bin/<mode>/Project.exe`. If no files have changed, then the binary is not re-compiled.

If the project is built as a library, then the `run` command will not execute the binary, and will instead output a
message saying that the project is a library.

| Flag        | Description                                                                                    |
|-------------|------------------------------------------------------------------------------------------------|
| `--debug`   | Compiles the project in debug mode. This will output the binary to the `bin/debug` folder.     |
| `--release` | Compiles the project in release mode. This will output the binary to the `bin/release` folder. |
| `--clean`   | Cleans the project before building. The `bin` folder is emptied, and the project is re-built.  |

The default command is `spp run --debug`.

### `spp clean`

The `clean` command is used to empty the `bin` folder. This will force the entire project to be re-built, as the binary
is deleted. The `bin` folder is not deleted, only emptied.

| Flag        | Description                               |
|-------------|-------------------------------------------|
| `--debug`   | Cleans the debug build.                   |
| `--release` | Cleans the release build.                 |
| `--all`     | Cleans both the debug and release builds. |

The default command is `spp clean --all`.
