---
layout: page
title: ccir
description:
img: assets/img/ccir1.jpeg
importance: 2
category: fun
---

My friend Kyle and I wrote a [tiny C compiler in rust (ccir)](https://github.com/KyleleeSea/ccir) over winter break. It supports: variables (local, global), function calls, control-flow, loops, among other features. The compiler adheres to the C11 standard, cdecl calling convention, and compiles to x86-64 bit assembly. This was a pretty fun way to learn Rust while also exploring how compilers work. Much of compiler was written by Kyle, and I only helped out a little bit. He's an amazing programmer, please check out [his GitHub](https://github.com/KyleleeSea)!


### Lexer

Our lexer expects a filepath, stores the contents as a string, then iterates over the string character-by-character. We initialize a token list and also create a set of tokens that represent various different elements the lexed program. We initialize an accumulator string which we append characters one-at-a-time to. If at any point the string represents a valid expression, keyword, etc in our program, we append the corresponding token at the end of the list along with necessary contextual information. For instance, if we come across something like:

```
if (0 == 1)
```

We append to our token list: `Token::TIf`, `Token::TIntLit(0)`, `Token::TEq`, `Token::TIntLit(1)`.

When the lexing is finished, we return the list of tokens the program has been parsed into. If we encounter any unexpected elements in the program, we raise an error.


### Parser

The parser takes in the token list from our lexer and constructs an abstract syntax tree (AST) from it. To keep our parser clean and easy to reason about, we kept it as functional as possible (with some mutation) and used [Backus-Naur Form](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form):

```
<function> ::= "int" <id> "(" [ "int" <id> { "," "int" <id> } ] ")" ( "{" { <block-item> } "}" | ";" )
```

This also helped us document code much more easily. Each function took the token stack and returned an ASTNode that handled implementing its own Grammar. Each function would call other parsing functions when it encountered certain tokens, and we had one top-level `parse_program` function that would take the token list created by the lexer and output the corresponding AST.


### Code Generation

The code generation structure is fairly similar to the parsing. We take in the root ASTNode and recurse down each child node. Depending on the tokens encountered, various different functions are called to process those subtrees. Processing some parts of the AST are tricky, especially when you need to "remember" certain things like: full registers, current location in the stack, declared variables and functions, etc. To handle things like variable and function declarations, we pass around `fn_validator` and `var_map`. `fn_validator` maps function IDs to their declaration and definition; `var_map` maps variable IDs to the register they currently occupy (or location on the stack) and value. This is just an example of one of the many things we'd had to implement for correct code generation. x86-64 assembly is written to `a.out`.

### Expanding the Compiler

We talked about expanding ccir in all sorts of ways. Some of the improvements we'd like to make:
- More (primitive) types
- Comments, Macros
- Arrays, Structs, Unions
- Dynamic Memory Allocation
- Register Allocation, optimizations
- Importing packages

Additionally, the error messages our compiler spits out currently aren't very helpful. In the future, we'd like to improve on this. Some other more interesting and unorthodox features we were thinking of adding:
- Contracts (like [Dafny](https://dafny.org/))
- Stack Canaries
- Classes, Inheritance


### Resources We Used
- [Nora Sandler's Writing a C Compiler](https://norasandler.com/archive/)
- [x86-64 calling convention](https://aaronbloomfield.github.io/pdr/book/x86-64bit-ccc-chapter.pdf)