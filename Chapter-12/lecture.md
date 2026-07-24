# Chapter 12 — Lecture: The C Preprocessor

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 12: "The C Preprocessor"

---

## 12.1 Features of the C Preprocessor

The **preprocessor** is a text-substitution tool that runs **before** actual compilation, processing every line beginning with `#` (a **directive**). It does not understand C syntax/semantics — it works purely on text.

| Feature | Directive |
|---|---|
| Macro expansion | `#define` |
| File inclusion | `#include` |
| Conditional compilation | `#if`, `#ifdef`, `#ifndef`, `#else`, `#elif`, `#endif` |
| Miscellaneous | `#undef`, `#pragma` |

## 12.2 Macro Expansion

### Simple (Object-like) Macros

```c
#define PI 3.14159
#define MAX_STUDENTS 120

float area = PI * radius * radius;   /* preprocessor replaces PI with 3.14159 BEFORE compilation */
```

- No semicolon after `#define` — it is a directive, not a C statement.
- By convention, macro names are written in **UPPERCASE** to visually distinguish them from ordinary variables.
- The substitution is **purely textual** — the compiler never even sees the name `PI`; it only sees `3.14159` after preprocessing.

### 12.2.1 Macros with Arguments (Function-like Macros)

```c
#define SQUARE(x) ((x) * (x))
#define MAX(a, b) ((a) > (b) ? (a) : (b))

int y = SQUARE(5);        /* expands to: ((5) * (5))  ->  25 */
int m = MAX(10, 20);      /* expands to: ((10) > (20) ? (10) : (20))  ->  20 */
```

> **Critical rule — always parenthesize macro parameters (and the whole expansion):** Without careful parenthesization, macros can silently produce wrong results due to operator precedence during textual substitution.

```c
#define SQUARE_BAD(x) x * x

int result = SQUARE_BAD(2 + 3);   /* expands to: 2 + 3 * 2 + 3 = 2 + 6 + 3 = 11 (WRONG! expected 25) */

#define SQUARE_GOOD(x) ((x) * (x))
int correct = SQUARE_GOOD(2 + 3); /* expands to: ((2 + 3) * (2 + 3)) = 25 (correct) */
```

### 12.2.2 Macros versus Functions

| Aspect | Macro (`#define`) | Function |
|---|---|---|
| Processing | Textual substitution before compilation | Actual compiled code, called at runtime |
| Speed | No function-call overhead (inlined by substitution) | Small call/return overhead |
| Type checking | **None** — purely textual, no type safety | Full type checking by the compiler |
| Debugging | Harder to debug (debugger sees expanded code) | Easier to step through |
| Side effects | Dangerous if arguments have side effects (e.g., `SQUARE(i++)` evaluates `i++` **twice**) | Safe — arguments evaluated exactly once |

```c
#define SQUARE(x) ((x) * (x))
int i = 5;
int result = SQUARE(i++);   /* expands to ((i++) * (i++)) -- i is incremented TWICE! undefined behaviour */
```

> This is a classic reason to prefer functions over macros for anything beyond the simplest textual constants — modern C also offers `inline` functions (advanced topic) that combine function safety with macro-like performance.

## 12.3 File Inclusion

```c
#include <stdio.h>     /* angle brackets: search SYSTEM/standard library directories */
#include "myheader.h"   /* double quotes: search the CURRENT directory first, then system dirs */
```

- `< >` is used for standard library headers.
- `" "` is used for your own project header files.
- `#include` textually pastes the entire contents of the named file at that point, before compilation proceeds.

## 12.4 Conditional Compilation

Conditional compilation lets you include or exclude blocks of code **based on conditions evaluated at compile time**, not at runtime.

```c
#define DEBUG 1

#if DEBUG
    printf("Debug mode: x = %d\n", x);
#endif
```

```c
#ifdef _WIN32
    printf("Compiling on Windows\n");
#elif __linux__
    printf("Compiling on Linux\n");
#else
    printf("Unknown platform\n");
#endif
```

### `#if` and `#elif` Directives

```c
#define VERSION 2

#if VERSION == 1
    /* code for version 1 */
#elif VERSION == 2
    /* code for version 2 */
#else
    /* fallback code */
#endif
```

### Header Guards — A Vital Use of Conditional Compilation

When writing multi-file programs, header files must be protected against being `#include`d multiple times (which would cause duplicate-definition errors):

```c
/* myheader.h */
#ifndef MYHEADER_H
#define MYHEADER_H

/* ... declarations ... */

#endif  /* MYHEADER_H */
```

- `#ifndef MYHEADER_H` — "if `MYHEADER_H` is **not** yet defined..."
- The first time this header is included, `MYHEADER_H` is undefined, so its contents are processed **and** `MYHEADER_H` gets `#define`d.
- On any subsequent `#include` of the same header (e.g., via multiple other headers that both include it), `MYHEADER_H` is already defined, so the entire body is skipped — preventing duplicate declarations.

## 12.5 Miscellaneous Directives

### `#undef`

Removes a previously defined macro, so it can be redefined differently later, or simply to signal it should no longer be used:

```c
#define MAX 100
/* ... use MAX ... */
#undef MAX
#define MAX 200   /* redefining after #undef avoids a "macro redefinition" warning */
```

### `#pragma`

Gives **compiler-specific** instructions that don't fit the standard directive categories (e.g., suppressing specific warnings, controlling structure packing). `#pragma` usage is inherently non-portable across different compilers.

```c
#pragma warning(disable: 4996)   /* MSVC-specific example */
```

## 12.6 The Build Process (Preprocessing in Context)

```
Source (.c) --[Preprocessor: expands #include, #define; strips comments]--> Translation Unit
           --[Compiler: syntax/semantic checks, code generation]--> Object code (.o)
           --[Linker: resolves external references, combines objects]--> Executable
```

You can view the preprocessed output directly using GCC:

```bash
gcc -E program.c -o program.i
```

This is useful for debugging tricky macro-expansion bugs — you can see exactly what code the compiler actually receives.

## 12.7 Worked Programs

### Program 1: Using Macros for Constants and Simple Computations

```c
#include <stdio.h>

#define PI 3.14159
#define AREA_OF_CIRCLE(r) (PI * (r) * (r))

int main()
{
    float radius = 5.0;
    printf("Area = %.2f\n", AREA_OF_CIRCLE(radius));
    return 0;
}
```

### Program 2: Conditional Compilation for Debug Logging

```c
#include <stdio.h>

#define DEBUG 1

int main()
{
    int x = 10, y = 20, sum;
    sum = x + y;

#if DEBUG
    printf("[DEBUG] x=%d, y=%d, sum=%d\n", x, y, sum);
#endif

    printf("Sum = %d\n", sum);
    return 0;
}
```

> Changing `#define DEBUG 1` to `#define DEBUG 0` removes the debug output entirely **at compile time**, with zero runtime overhead — a key advantage of conditional compilation over runtime `if` checks for debug logging.

## 12.8 Key Takeaways

1. The preprocessor performs purely textual substitution before actual compilation begins — it has no understanding of C syntax or types.
2. `#define` creates macros; always parenthesize both parameters and the entire macro body to avoid precedence bugs.
3. Macro arguments with side effects (like `i++`) can be evaluated multiple times, causing subtle bugs — functions don't have this problem.
4. `#include <...>` searches system directories; `#include "..."` searches the local project first.
5. Conditional compilation (`#ifdef`, `#if`, `#ifndef`) selects code at compile time — essential for header guards and platform-specific/debug builds.
