# 1.3. Project Structure

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Project Structure

An SPP Project has certain folders and files required for the project to be built and run. The following is a list of
the required folders and files, that must reside in the project root directory - assume this project folder is called
`Project`.

- `src` - The source code of the project. This folder contains all `.spp` files that are part of `Project`. When the
  project is built, the compiler will compile all `.spp` files in this folder. Deleting this folder will cause the S++
  compiler to throw an error at compile time. Note that the `main.spp` file (detailed below), must reside in the root of
  the `src` folder, not the project root.
- `bin` - The binary output of the project. This folder contains the compiled binary of the project. When the project is
  built, the compiler will output the binary to this folder. Deleting this folder will not break the project, but it
  will be regenerated automatically on the next build. Running the `clean` command will empty, and not delete, this
  folder.
- `ffi` - The FFI folder contains externally compiled code, conforming to the C ABI, that can be imported into the
  project. This folder is optional, and the compiler will not complain if it is not present. The `ffi` folder will
  contain subfolders of projects, that will in turn contain stubs and dlls of the external code. The compiler will
  automatically link the external code when it is imported into the project. See the [FFI](13-2-FFI.md) topic for more
  information.
- `vcs` - The version control system folder contains other non-compiled S++ projects that can be compiled into the
  current project. This folder is optional, and the compiler will not complain if it is not present. The `vcs` folder
  will contain subfolders of projects, that will in turn contain the source code of the external projects.
- `ext` - The external folder is identical to the `vcs` folder, but it contains non-vcs code. This is for when projects
  are shared online, but not through a version control system.
- `tst` - The test folder contains the tests for the project. **TODO.**
- `main.spp` - The main file of the project. This file must reside in the root of the `src` folder, not the project
  root. This file is the entry point of the project, and must contain a `main` function, which is the entry point of the
  project. The `main` function must be declared as `fun main(args: Vec[Str]) -> std::Void { ... }`.
- `spp.toml` - The project configuration file. This file contains metadata about the project, such as the project name,
  version, and dependencies. It resides in the project root. This file is required for the project to be built. The
  `spp.toml` file must reside in the root of the project folder. It is auto-generated when `spp new` is run, and can be
  manually edited to add dependencies and other metadata.

## Version Control System & External Projects

By keeping individual `vcs/ext` folders per project, it mirrors Python's virtual environment system, where each project
has its own dependencies.

When an S++ project is run, the vcs folders are checked for updated versions of the projects, unless the `--no-update`
flag is added to the compilation. If there is a newer version of the repository, an `info` message will be displayed
notifying the user that there is a newer version available, but the project will still compile with the current version.

## Project Structure Example

The following is an example of a project structure:

```
Project
├── bin
│   └── Project.exe
├── src
│   ├── main.spp
│   ├── folder1
│   │   ├── file1.spp
│   │   └── file2.spp
│   └── folder2
│       ├── file3.spp
│       └── file4.spp
├── ffi
│   └── v8
│       ├── v8.dll
│       └── stub.spp
├── vcs
│   └── web_parser
│       ├── file1.spp
│       ├── file2.spp
│       └── main.spp
└── spp.toml
```

## Configuration File

The `spp.toml` configuration file is a TOML file that contains metadata about the project. The following is an example
of a `spp.toml` file:

```
[project]
name = "Project"
version = "0.1.0"
description = "A project that demonstrates the project structure."
authors = ["Author Name"]
license = "MIT"

[vcs]
std = { git = "https://github.com/spp-lang/std", branch = "main" }
```

A default project will contain the above sections and attributes. From the `project` section, only the `name` and
`version` attributes are required. The `vcs` section is optional, and by default it will import the standard library
into the VCS folder.

The standard library repository has a new branch per version, but keeping it on `main` will automatically update the
code to the latest version. Projects that are barebones must keep the `stl` dependency, but use the `@no_std`
annotation, which will _reduce_ the STL down to the bare minimum / core functionality.
