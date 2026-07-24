# Chapter 12 — Quiz: The C Preprocessor

## 📖 Topics Covered: `#define` macros, macro pitfalls, `#include`, conditional compilation, header guards

---

## Part A: Multiple Choice Questions (5)

### Q1. What does `int result = SQUARE(2 + 3);` expand to and evaluate as, given `#define SQUARE(x) x * x`?

A) `((2+3)*(2+3))`, evaluating to `25`
B) `2 + 3 * 2 + 3`, evaluating to `11`
C) `5 * 5`, evaluating to `25`
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**B) `2 + 3 * 2 + 3`, evaluating to `11`**

Since `SQUARE(x)` is defined as `x * x` **without parentheses**, the textual substitution literally replaces `x` with `2 + 3` everywhere, producing `2 + 3 * 2 + 3`. By operator precedence, this evaluates as `2 + (3*2) + 3 = 2+6+3 = 11`, not the intended `25`.
</details>

---

### Q2. What is the correct, safe way to define a macro that squares its argument?

A) `#define SQUARE(x) x * x`
B) `#define SQUARE(x) ((x) * (x))`
C) `#define SQUARE(x) (x * x);`
D) `#define SQUARE(x) = x * x`

<details>
<summary><b>Answer</b></summary>

**B) `#define SQUARE(x) ((x) * (x))`**

Wrapping both the parameter `x` and the entire expression in parentheses ensures correct behaviour regardless of what expression is passed in or what surrounds the macro call, avoiding precedence-related substitution bugs. (Option C incorrectly includes a trailing semicolon, which would be textually inserted wherever the macro is used, potentially causing syntax errors.)
</details>

---

### Q3. What is the difference between `#include <stdio.h>` and `#include "myheader.h"`?

A) There is no difference; both search the same directories in the same order
B) Angle brackets search system/standard library directories; quotes search the local project directory first
C) Angle brackets are for C++, quotes are for C
D) Quotes only work for `.c` files, not `.h` files

<details>
<summary><b>Answer</b></summary>

**B) Angle brackets search system/standard library directories; quotes search the local project directory first**

`< >` is conventionally used for standard/system headers, telling the preprocessor to look in standard include paths. `" "` is used for your own project's headers, causing the preprocessor to look in the current directory (and project-specific paths) first before falling back to system directories.
</details>

---

### Q4. What is the purpose of a "header guard" like `#ifndef HEADER_H ... #define HEADER_H ... #endif`?

A) To improve program runtime speed
B) To prevent a header file's contents from being processed more than once if included multiple times, avoiding duplicate-definition errors
C) To hide the header's contents from other files entirely
D) To automatically document the header file

<details>
<summary><b>Answer</b></summary>

**B) To prevent a header file's contents from being processed more than once if included multiple times, avoiding duplicate-definition errors**

If the same header ends up being `#include`d more than once (directly or indirectly through other headers), a header guard ensures its body is only actually processed the first time, since the guard macro becomes defined after that first inclusion.
</details>

---

### Q5. Given `#define SQUARE(x) ((x) * (x))` and `int i = 5; int r = SQUARE(i++);`, what problem occurs?

A) No problem; `r` correctly becomes `25` and `i` becomes `6`
B) `i++` is textually substituted and therefore evaluated twice, causing undefined behaviour
C) Compilation error, since macros cannot accept expressions with `++`
D) `r` becomes `0` because `i++` returns void

<details>
<summary><b>Answer</b></summary>

**B) `i++` is textually substituted and therefore evaluated twice, causing undefined behaviour**

The macro expands to `((i++) * (i++))`. Since `i++` appears twice in the expanded text, it is evaluated (and `i` incremented) twice within the same expression — a classic macro pitfall not present with ordinary functions, where each argument is evaluated exactly once.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain why the C preprocessor is described as performing "purely textual substitution," and what limitation this creates compared to using a real function.

<details>
<summary><b>Model Answer</b></summary>

The preprocessor operates as a **text-processing pass** that runs *before* the actual C compiler ever sees the code — it has no knowledge of C's grammar, types, or semantics. When it encounters a macro invocation, it simply replaces the macro name (and its parameter placeholders) with the corresponding replacement text, character for character, exactly as written in the `#define`.

**Limitation:** Because there is no type-checking or semantic understanding involved, macros provide **no type safety** (you can pass anything textually substitutable, and mistakes are only caught, if at all, once the *expanded* code is compiled) and can produce **surprising results** if precedence or multiple-evaluation issues (like `SQUARE(i++)`) are not carefully avoided by the macro's author. Ordinary functions, by contrast, are checked by the compiler for argument types and evaluate each argument exactly once, avoiding both of these classes of bugs.
</details>

---

### Q2. Rewrite the following buggy macro so that it behaves correctly for any expression passed as an argument, and explain each change.

```c
#define DOUBLE(x) x + x
```

<details>
<summary><b>Model Answer</b></summary>

```c
#define DOUBLE(x) ((x) + (x))
```

**Explanation of changes:**
- Wrapping the parameter `x` in parentheses — `(x)` — ensures that if the caller passes a compound expression (e.g., `DOUBLE(2 + 3)`), it is treated as a single unit during substitution: `((2 + 3) + (2 + 3))` rather than the unparenthesized `2 + 3 + 2 + 3`, which could interact incorrectly with surrounding operators depending on context.
- Wrapping the *entire* macro body in an outer set of parentheses ensures that if `DOUBLE(...)` itself appears inside a larger expression with other operators (e.g., `int y = 10 * DOUBLE(3);`), the whole macro's result is treated as one unit before combining with the rest of the expression, again avoiding precedence surprises.
</details>

---

### Q3. Describe the sequence of events (with reference to `#include`, `#define`, and comments) that happens when the preprocessor processes a `.c` file, before the compiler proper even begins.

<details>
<summary><b>Model Answer</b></summary>

1. **Comment removal:** All `/* ... */` and `//` comments are stripped from the source text.
2. **File inclusion (`#include`):** Every `#include` directive is replaced by the *entire textual contents* of the named header file, recursively (if that header itself has `#include`s, those are expanded too).
3. **Macro expansion (`#define`):** Every occurrence of a previously `#define`d macro name in the remaining code is textually substituted with its replacement text (and, for function-like macros, with arguments substituted into the parameter placeholders).
4. **Conditional compilation resolution:** `#if`/`#ifdef`/`#ifndef`/`#else`/`#elif`/`#endif` blocks are evaluated, and only the selected branch's code is retained in the output; the rest is discarded entirely before the compiler ever sees it.

The result of all these steps is a single, fully expanded "translation unit," which is what the actual C compiler then parses, type-checks, and translates into object code.
</details>

---

### Q4. Compare the use of `#define` macros versus `const` variables/functions for defining a constant like `PI`. Which is generally recommended in modern C, and why?

<details>
<summary><b>Model Answer</b></summary>

```c
#define PI 3.14159        /* macro: pure textual substitution, no type, no scope */
const double PI = 3.14159; /* const variable: has a real type, occupies memory, respects scope rules */
```

A `#define`d constant has **no type** of its own (the preprocessor just inserts the literal text `3.14159` everywhere `PI` appears) and is **not visible to the debugger** as a named entity — a debugger sees only the expanded literal value, making it harder to inspect during debugging. It also completely ignores C's scoping rules, since it is a preprocessor-level substitution, not a real C declaration.

A `const` variable, in contrast, is a genuine, typed C entity: the compiler enforces its type, it respects normal scope rules (it can be local to a function or file), and debuggers can display it by name during a debugging session.

**Recommendation:** Modern C style generally prefers `const` (or `enum` for integer constants) over `#define` for simple constant values, reserving macros mainly for things that genuinely require textual substitution (like header guards or conditional compilation), because `const` provides real type safety and better debuggability.
</details>

---

### Q5. Explain, step by step, why a header guard prevents "duplicate definition" compiler errors when a header is (perhaps indirectly) included more than once.

<details>
<summary><b>Model Answer</b></summary>

Consider a header `shapes.h`:
```c
#ifndef SHAPES_H
#define SHAPES_H
struct Circle { double radius; };
#endif
```

Suppose two different headers, `a.h` and `b.h`, both `#include "shapes.h"`, and a single `.c` file includes both `a.h` and `b.h`.

- **First inclusion (via `a.h`):** `SHAPES_H` is not yet defined, so the preprocessor executes the `#ifndef` block: it immediately `#define`s `SHAPES_H`, and then processes (keeps) the `struct Circle` declaration.
- **Second inclusion (via `b.h`, in the same translation unit):** By now `SHAPES_H` **is** already defined (from the first inclusion), so `#ifndef SHAPES_H` evaluates false, and the preprocessor **skips** the entire body between `#ifndef` and `#endif` — meaning `struct Circle` is **not** declared a second time.

Without this guard, `struct Circle` (and any other declarations in the header) would be textually pasted into the final translation unit twice, and the compiler would correctly reject this as a duplicate-definition error.
</details>
