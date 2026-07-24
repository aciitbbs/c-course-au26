# Chapter 1 — Quiz: Getting Started

## 📖 Topics Covered: C history, character set, constants, variables, keywords, `main()`, `printf`, `scanf`, compilation model

---

## Part A: Multiple Choice Questions (5)

### Q1. Who developed the C programming language, and where?

A) James Gosling at Sun Microsystems
B) Bjarne Stroustrup at Bell Labs
C) Dennis Ritchie at Bell Telephone Laboratories
D) Guido van Rossum at CWI

<details>
<summary><b>Answer</b></summary>

**C) Dennis Ritchie at Bell Telephone Laboratories**

Dennis Ritchie created C in 1972 at Bell Labs (AT&T), largely to help rewrite the UNIX kernel. Gosling created Java, Stroustrup created C++, and van Rossum created Python.
</details>

---

### Q2. Which of these is an *invalid* variable name in C?

A) `_marks2026`
B) `Roll_No`
C) `2ndYear`
D) `totalMarks`

<details>
<summary><b>Answer</b></summary>

**C) `2ndYear`**

A C identifier must begin with a letter or an underscore — never a digit. All the others are valid: identifiers may contain digits and underscores after the first character, and C is case-sensitive so `Roll_No` is fine.
</details>

---

### Q3. What is stored internally when you write `char ch = 'A';`?

A) The two characters `'` and `A`
B) The string `"A"` with a null terminator
C) The integer ASCII code 65
D) A pointer to the letter A

<details>
<summary><b>Answer</b></summary>

**C) The integer ASCII code 65**

Character constants are stored as their integer ASCII codes internally. `'A'` is 65, `'a'` is 97, and `'0'` (the character) is 48. `printf("%d", ch)` on this variable would print `65`.
</details>

---

### Q4. Which statement about `scanf()` is correct?

A) The `&` operator is optional and only used for style
B) `scanf()` requires `&` before scalar variables to know the memory address to store input into
C) `scanf()` can only read integers
D) `scanf()` does not need a header file

<details>
<summary><b>Answer</b></summary>

**B) `scanf()` requires `&` before scalar variables to know the memory address to store input into**

`&` is the address-of operator. Omitting it before a scalar (non-array) variable is one of the most common beginner bugs and leads to undefined behaviour or crashes. `scanf` also needs `#include <stdio.h>` and can read many types (`%d`, `%f`, `%c`, `%s`, etc.).
</details>

---

### Q5. Consider the compilation pipeline. Which stage resolves a call to the library function `printf()` by linking in the C standard library?

A) Preprocessing
B) Compilation
C) Linking
D) Execution

<details>
<summary><b>Answer</b></summary>

**C) Linking**

Preprocessing expands directives like `#include`; compilation translates source into object code (`.o`), possibly leaving external references (like `printf`) unresolved; linking combines your object code with library code to produce a complete executable.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. List and briefly explain the three main stages a C source file passes through before it becomes a runnable program.

<details>
<summary><b>Model Answer</b></summary>

1. **Preprocessing** — The preprocessor scans the source file for lines beginning with `#` (directives). It textually expands `#include` files, substitutes `#define` macros, and strips out comments, producing an expanded, pure-C translation unit.
2. **Compilation** — The compiler performs lexical, syntax, and semantic analysis on the preprocessed code and translates it into machine-specific **object code** (a `.o`/`.obj` file). References to functions defined elsewhere (e.g., in a library) are left as unresolved symbols.
3. **Linking** — The linker combines one or more object files together with required library code (e.g., the implementation of `printf` inside the C standard library) to resolve all external references and produce a single, executable binary.

Running `gcc program.c -o program` performs all three stages transparently in one command.
</details>

---

### Q2. Explain the difference between a *constant*, a *variable*, and a *keyword* in C, giving one example of each.

<details>
<summary><b>Model Answer</b></summary>

- A **constant** is a fixed value written directly into the source code that never changes during execution — e.g., `100` (integer constant), `3.14` (real constant), or `'A'` (character constant).
- A **variable** is a *named memory location* whose stored value can change while the program runs — e.g., `int marks = 80;` followed later by `marks = 95;`.
- A **keyword** is a reserved word that already has a fixed, special meaning to the compiler, such as `int`, `return`, or `while`. Keywords cannot be redefined or used as variable/function names.

In short: constants are fixed literal values, variables are containers whose contents may vary, and keywords are part of the language's own vocabulary.
</details>

---

### Q3. Write down the rules for constructing a valid variable name in C, and identify which rule is broken in each of these: `1data`, `total marks`, `float`.

<details>
<summary><b>Model Answer</b></summary>

**Rules:**
1. Must begin with a letter (A–Z, a–z) or an underscore `_`.
2. May contain only letters, digits, and underscores after the first character.
3. Cannot contain blanks, commas, or other special symbols.
4. Cannot be a C keyword.
5. C is case-sensitive.

**Violations:**
- `1data` — starts with a digit (violates rule 1).
- `total marks` — contains a blank space (violates rule 3).
- `float` — is a reserved keyword (violates rule 4).
</details>

---

### Q4. What is a "garbage value"? Why should you always initialize variables before using them? Illustrate with a short code snippet.

<details>
<summary><b>Model Answer</b></summary>

A **garbage value** is whatever bit pattern happens to already exist in a memory location at the time a variable is declared but not yet assigned a value. C does **not** automatically initialize local variables to zero (unlike some other languages), so reading an uninitialized variable yields unpredictable, meaningless data that can differ across runs, compilers, or machines.

```c
#include <stdio.h>
int main()
{
    int total;                 /* not initialized */
    printf("%d\n", total);     /* prints a garbage value, e.g., some random integer */

    total = 0;                 /* now properly initialized */
    printf("%d\n", total);     /* reliably prints 0 */
    return 0;
}
```

Always initializing variables avoids subtle, hard-to-reproduce bugs that depend on whatever happened to be left in memory.
</details>

---

### Q5. Differentiate between the `%d`, `%f`, `%c`, and `%s` format specifiers used with `printf()`/`scanf()`, giving an example call for each.

<details>
<summary><b>Model Answer</b></summary>

| Specifier | Used for | Example |
|---|---|---|
| `%d` | `int` (signed decimal integer) | `printf("%d", 42);` → `42` |
| `%f` | `float` (defaults to 6 digits after the decimal point) | `printf("%f", 3.5);` → `3.500000` |
| `%c` | a single `char` | `printf("%c", 'A');` → `A` |
| `%s` | a string (`char` array terminated by `'\0'`) | `printf("%s", "Hi");` → `Hi` |

When reading with `scanf`, the same specifiers are used, but each scalar variable name must be preceded by `&` (e.g., `scanf("%d", &marks);`) — except for strings read with `%s`, where the array name itself already represents an address, so no `&` is used (e.g., `scanf("%s", name);`).
</details>
