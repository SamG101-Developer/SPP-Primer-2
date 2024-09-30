# 6.1. Module System

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Module System

A module is synonymous with a "package" in Java. Each folder is a module, and the files inside the folder make up the
module. A submodule is a module nested within another module, ie a folder within a folder. The namespacing of types and
functions matches the module layout, which in turn matches the directory layout. Because a **folder**, not the file, is
the module, the namespace doesn't include the filename.

For example, `Str` is defined in `std/string.spp`, but is namespaced as `std::Str`. This reduces namespace cluttering.
