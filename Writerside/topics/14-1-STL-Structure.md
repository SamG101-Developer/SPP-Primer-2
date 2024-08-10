# 14.1. STL Structure

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Layered Structure

The STL is split into 4 layers:
1. Core Layer
2. Platform Abstraction Layer
3. Foundation Layer
4. Extended Layer

### Core Layer

The core layer is the most essential part of the STL, and must even be present in a `no-std` environment. It includes
the absolute minimum functionality necessary to support a bare-bones runtime environment. This includes, and is limited
to low level allocation and de-allocation, mock primitive types, essential data structures, minimal compiler-known
stateless functionality classes, aborting and concurrency primitives:

- **Allocation**: The `Alloc` class is the wrapper around manual memory management. Because only arrays are used for
  allocation, the `Alloc::allocate` method returns an array of empty elements. Bounds checking in the `Arr` class is
  used to ensure empty indexes aren't accessed.

- **Mock Primitive Types**: The mock primitive types include the `Bool` class, primitive numeric classes (signed,
  unsigned and floats, 8 to 256 bites), and the `Void` class. These are used to provide a basic set of primitive types
  to the compiler.

- **Essential Data Structures**: The core layer includes the `Arr` class, which is a fixed size array. The other core
  classes required are the `Opt` and `Res` types, which are used for optional and result types respectively. All of
  these types are used in the core layer to provide the basic data structures required for the STL.

- **Functionality Classes**: The `Copy`, `Clone`, and `std::ops::*` classes are required for the number types
  especially, and to opt out of move-only semantics where required.

- **Atomic Primitives**: The `Atomic[T]` type is required for atomic operations. Other threading primitives are in the
  Platform Abstraction Layer. The `Volatile[T]` type is also required for volatile operations.

- **Asynchronous Primitives**: The `Fut[T]` type is required for asynchronous operations, and as asynchronous functions
  are first-class, the `Fut` type is required in the core layer.

### Platform Abstraction Layer

The platform abstraction provides platform-specific functionality over a uniform set of features, allowing the same code
to be run on different platforms. This means that the core layer can be platform-agnostic, and the platform abstraction
layer can be used to provide the necessary platform-specific functionality. This includes:

- **Platform Specific IO**: Console, file and networking IO are all provided in the platform abstraction layer. These io
  modules use conditional compilation to provide the correct functionality for each platform.

- **Threading Primitives**: The `Mutex`, `CondVar`, and `Thread` classes are all provided in the platform abstraction
  layer. They hook into the platform-specific threading API to utilize the correct threading primitives.

- **Time**: The `Time` class is provided in the platform abstraction layer. It provides a way to get the current time,
  and to sleep for a certain amount of time.

### Foundation Layer

The foundation layer provides the basic building blocks for the STL. This includes the `Str` class, all the containers,
and other basic structures. The foundation layer doesn't typically use the platform abstraction layer, but does use the
core layer. The foundation layer includes:

- **Containers**: There is a wide range of containers in the foundation layer, including the commonly
  used `Vec`, `Map` and `Set` classes. Classes such as the `Stack` and `Queue` classes have a `Backend` generic type
  parameter that allows the user to choose the underlying data structure.

- **Strings**: The `Str` class is the string class in the foundation layer. It also has a `Backend` generic type
  parameter that provides the core functionality of the string, such as `RopeStr` or `VecStr`. The `Str` class's more
  comprehensive methods will in turn call teh fixed set of backend methods.

- **Iterators**: The `IterMov`, `IterMut`, `IterRef` class is the iterator class in the foundation layer. Each iterator
  class superimposes the corresponding generator class, allowing a yield-based approach to iteration (internal
  iteration). All the utility method (filtering, mapping, etc.) are provided here.

- **Big Number & Decimal**: The `BigInt` and `BigDec` classes are provided in the foundation layer. These classes are
  used for arbitrary precision arithmetic, and are only limited by the available memory. A comprehensive math library,
  following IEEE 754, is provided for number-related classes, using constrained generics to re-use code.

### Extended Layer

The extended layer provides more advanced functionality, building on the foundation layer. This includes things such as
file systems, networking with TCP/UDP/HTTP, and reflection. These are libraries that are not required for the core
functionality of the STL, but are useful for more advanced applications.

There are a number of libraries in the extended layer, under `std::libs::httplib` for example, so
that `use std::libs::httplib` can be used to do `httplib::get` for example. This allows the libraries to be used in a
modular way, with clear imports to show it belongs to the STL.

