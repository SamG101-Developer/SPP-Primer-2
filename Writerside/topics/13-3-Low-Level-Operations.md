# 13.3. Low Level Operations

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Low Level Operations

S++ is designed to operate completely independently of any other languages, including C, in order to simplify the
implementation of the compiler and runtime. This means that S++ must use its own techniques to manage ultra-low-level
operations, such as io, time, random number generation, and threading.

## Implementation

In order to achieve true independence from C, S++ implements its own LLVM code which handles these low-level operations,
with platform specific implementations for each of the supported platforms. There are three stages to the low level
operations:

1. S++ functions. These include things such as `print`, `read`, `clear`. They call the second stage functions with the
   correct arguments, such as converting strings to arrays etc.
2. S++ LLVM stubs. These are the function signatures that represent the LLVM IR functions. They are connected to the
   linked library "spp-llvm" which contains the actual implementations.
3. LLVM IR functions. These are the actual implementations of the functions, which are written in LLVM IR and compiled
   into the `spp-llvm` library.

```llvm
define void @console_print(i8* %input, i64 %length) {
    %1 = call i64 asm "syscall", "=r,{rax},{rdi},{rsi},{rdx}"(i64 1, i64 1, i8* %input, i64 %length)
    ret void
}

define i64 @console_read(i8* %output, i64 %length) {
    %read = call i64 asm "syscall", "=r,{rax},{rdi},{rsi},{rdx}"(i64 0, i64 0, i8* %output, i64 %length)
    ret i64 %read
}

define void @console_clear() {
    %1 = call i64 asm "syscall", "=r,{rax}"(i64 2)
    ret void
}
```

```
mod std::io::console

fun print(message: Str) -> Void {
    llvm::console_print(message.as_arr(), message.length())
}

fun read() -> Str {
    let buffer = Arr[I8](length=256)
    let read = llvm::console_read(&mut buffer, 256)
    ret Str::from_arr(buffer)
}

fun clear() -> Void {
    llvm::console_clear()
}
```