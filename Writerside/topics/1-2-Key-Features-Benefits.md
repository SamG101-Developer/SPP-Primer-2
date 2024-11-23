# 1.2. Key Features &amp; Benefits

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Language Features

1. **Expression Oriented**: In S++, almost every "statement" is an expression. This includes conditional branching,
   conditional looping, contextual blocks, etc. There are a select number of statements that strictly modify symbols
   rather than return a value, such as variable declarations and assignments.
2. **Advanced Type System**: All types in S++ are first-class. This includes functions, tuples, arrays, numbers,
   booleans and voids. Union and intersection types are supported, as well as generic and variadic types. Flow typing
   can be used with union types for more precise type checking within branches. Other key type-oriented features include
   type shorthands, type inference, and type aliasing.
3. **Memory Safety**: S++ follows Rust in having a strong emphasis on memory safety. It uses a blend of: move semantics;
   ownership tracking; second-class borrows; and the Law of Exclusivity; to ensure that memory is managed safely and
   efficiently, mitigating the risk of memory bugs commonly found in other low-level languages.
4. **Class System**: Inspired by Rust, the class system in S++ is also split into state and behaviour. Each class
   definition includes state only (attributes). All methods and inheritance are defined using superimposition. This
   allows for a more flexible and powerful class system, where classes can be combined and extended in a more modular
   way.
5. **Module System**: Module definitions, namespaces and directory structures are all the same in S++. Every folder is a
   nested module identifier and therefore namespace, heavily simplifying the module system. This contrasts with C++,
   where the directory structure, module identifiers and namespacing are all separate concepts and can be different.
6. **Functions**: Functions and methods can be overloaded, have default and variadic arguments, and can return multiple
   values by using a tuple and variable destructuring. They can be passed as arguments to other functions due to their
   first-class nature. Closures can be defined and can capture variables from their enclosing scope. Recursive functions
   are optimized by the compiler to be tail-recursive.
7. **Generics**: S++ has a powerful generics system, supporting generic functions, types, and methods. Generic types can
   be constrained using where clauses, and generic functions can be overloaded based on the constraints. Generic types
   can also be variadic, allowing for a variable number of different type arguments.
8. **Asynchronous functions**: S++ uses Golang like syntax for asynchronous functions, where the asynchronous convention
   is defined at the function call site, not the function definition. This mitigates the "function color" problem,
   removes the need for separate synchronous and asynchronous functions, and allows for reusable code.
9. **Concurrency**: Coroutines form the concurrency model in S++. They can be suspended and resumed, and support
   bilateral data movement in and out of the coroutine. They combine with the generator return types, providing a
   first-class way to advance generators and pass data. Borrows can be yielded from coroutines, supporting the basis of
   iterators.
10. **Parallelism**: S++ uses threads and their associated primitives, such as mutexes, condition variables, and
    semaphores, for parallelism. This is pulled from C++, and allows for parallel execution of code. The APIs have been
    modernized to fit the S++ language. Low level C libraries will be interfaced with the FFI for cross-platform
    compatibility.
11. **Error Handling**: S++ uses a result-type for error handling, similar to Rust. This allows for more expressive
    error handling. The result type can be used with the `?` operator to propagate errors up the call stack, and allow
    handling of the error in a parent function.
12. **Pattern Matching**: S++ has a powerful pattern matching system, similar to Rust. This allows for complex
    destructuring of tuples, unions, and objects, and can be used in conjunction with flow typing for more precise type
    checking. Patterns can be used in match statements, function parameters, and variable declarations.
13. **Loop Control Flow**. S++ provides the ability to skip an iteration of a loop or exit it early. By pairing a value
    with a loop exit, a value can be returned from a loop, allowing it to be used as an expression. Multiple loops can
    be exited from, without labels.
14. **Contextual Blocks**: Like Python, S++ supports context blocks, allowing code to be executed before and after a
    block of code. This is useful for resource management, such as file handling, where the file can be closed after the
    block of code has executed.
15. **Operator Overloading**: S++ supports operator overloading, allowing for operators to be defined for custom types.
    This is limited to the binary operators and the `not` operator, as S++ doesn't have unary operators. An example is
    multiplying strings by a number to repeat the string.
