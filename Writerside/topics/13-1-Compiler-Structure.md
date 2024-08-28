# 13.1. Compiler Structure

<primary-label ref="header-label"/>

<secondary-label ref="doc-wip"/>

## Compiler Structure

S++ uses a multi-stage compiler, split into the following stages:
- **Lexical Analysis**: The lexer reads the source code and converts it into a stream of tokens.
- **Syntactic Analysis**: The parser reads the stream of tokens and converts it into an abstract syntax tree (AST).
- **Preprocessing**: The preprocessor reads the AST and performs various transformations.
- **Symbol Generation**: The symbol generator reads the AST and generates a symbol table.
- **Sup Scope Loading**: The sup scope loader reads the AST and loads the superclasses of each class.
- **Semantic Analysis**: The semantic analyzer reads the AST and performs various semantic checks.
- **Code Generation**: LLVM IR code is generated from the AST.
- **Executable or Library**:  The machine code is linked and converted into an executable or library.

### Lexical Analysis

The lexer reads the source code and using both regex and equality comparisons, converts it into a stream of tokens. The
lexer is also responsible for handling comments and whitespace. Comments are removed from the source code during lexical
analysis; whitespace and newlines are retained for line termination.

### Syntactic Analysis

The parser reads the stream of tokens and converts it into an abstract syntax tree (AST). The parser is responsible for
ensuring that the source code is syntactically correct. If the parser encounters a syntax error, it will attempt to
recover and continue parsing with a valid alternative path. If the parser cannot recover, it will raise a syntax error.

### Preprocessing

The preprocessor makes various transformations to the AST, before any symbols are generated. This includes mapping
functions to function class superimpositions, and conditional compilation.

### Symbol Generation

Symbol generation focuses on module and superimposition member generation. It reads the AST and generates a symbol table
for classes, functions, methods and superimpositions. This does not include the contents of the functions. This allows
circular imports and any order of declaration, as the symbol table is generated before any code tries to reference it.

### Sup Scope Loading

There is a specific stage for loading the superclasses of each class. This is because all the types must be loaded
before their scopes are linked as superclasses, but must occur before any function bodies are analysed, as they may
reference symbols of superclasses.

### Semantic Analysis

The semantic analyzer reads the AST and performs various semantic checks. This includes moving into function bodies, and
analysing all code. Type inference, type checking, name resolution and all associated checks are performed in this
stage. This is the most complex stage of the compiler, as it must handle all the complex rules of the language.

### Code Generation

The code generator reads the AST and generates the target code. This uses LLVM as the backend, and generates LLVM IR
code. This is then compiled to machine code by the LLVM compiler.

### Executable or Library

The machine code is linked and converted into an executable or library. This is the final stage of the compiler, and
produces the final output that can be run on the target machine.
