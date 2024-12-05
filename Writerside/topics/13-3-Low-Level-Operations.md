# 13.3. Low Level Operations

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Low Level Operations

S++ utilizes `libc` and `pthreads` for low-level operations, such as io, time, random number generation, and threading.
This allows S++ to be platform independent, as `libc` is available on all platforms. The library file in the folder must
be of the format: `<lib_name>-<platform>-<arch>.<ext>`, where `<platform>` is the platform name, `<arch>` is the
processor architecture, and `<ext>` is the file extension. For example:
- `libc-win32-amd64.dll` for `libc` on Windows, running on an `x64` processor.

Only the correct library for the platform will be loaded, and the correct functions will be called. This means that
whilst the other versions won't be used, for the sake of distribution, they are included in the project.

### Libraries Pre-Loaded
- `libc` for general low-level operations.
- `pthreads` for threading operations.
- `socket` for network operations.

## Implementation

In order to interop with `libc`, S++ uses standard library linking techniques to call the C code. There are three stages
to the low level operations:

1. S++ functions. These include things such as `print`, `read`, `clear`, and form the STL API. They call the second
   stage functions with the correct arguments, such as converting strings to arrays etc.
2. S++ C stubs. These are the function signatures that represent the C functions. They are connected to the linked
   library "spp-libc" which contains the actual implementations.
3. C functions (in the `libc`library). These are the actual implementations of the functions, which are written in C and
   compiled into the `spp-llvm` library.

## Issues with `libc`

There are some issues with `libc` that are addressed within the S++ binding API, so that `libc` cannot throw certain
errors.

- **Buffer Overflow**: Some functions in `libc` can cause buffer overflows, the most well-known being:
    - functions `strcpy`, `strcat`: lack of bounds checking.
    - functions `gets`, `scanf`: lack of input length checking.
    - string routines not always guaranteed to null-terminate.
    - function `printf`: can cause buffer overflows if the format string doesn't match the arguments.

  All these security vulnerabilities are mitigated by introducing safe, wrapped versions of the methods, at the S++
  level. That is, instead of direct argument conversion and calling the `libc` code, extra checks are performed. Because
  the `libc` code can throw errors, the S++ wrappers return `Res` types, so that the error can be handled in S++, and
  unexpected behaviour can be handled.

- **Thread Safety**: `libc` functions are not always thread-safe.

## Example

```
fun print(string: Str) -> Res[I32, CError] {
    let arr = Arr[U8]::from(string)
    let res = libc::printf(&arr)
    res.map_err(|_| CError::new("Failed to print"))
    
    ret case res >= 0 { Pass(value=res) }
    else { Fail(error=CError::new("Failed to print")) }
}

fun read() -> Res[Str, CError] {
    let mut buffer = Arr[U8](length=256)
    let res = libc::scanf(&mut buffer)
    res.map_err(|_| CError::new("Failed to read"))
    
    ret case res >= 0 { Pass(value=Str::from(buffer)) }
    else { Fail(error=CError::new("Failed to read")) }
}
```

```
fun printf(&arr: Arr[U8]) -> I32 { }
fun scanf(buffer: &mut Arr[U8]) -> I32 { }
```

The FFI system will [convert](13-2-FFI.md#conversions) the `&Arr[U8] @SPP` type to `const U8* @C`,
and `&mut Arr[U8] @SPP` to `U8* @C`. The results from C will be `int @C`, which will be converted to `I32 @SPP`.

## Other platforms

While `libc` is the standard C library for Unix-like systems, Windows uses other libraries. It was decided that
the `UCRT` library would be used for Window's low level operations linking, because it uses the same api as `libc`. This
means the same set of stubs can be used for both platforms, and the same S++ code can be used on both platforms.
