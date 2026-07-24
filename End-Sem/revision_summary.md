# End-Semester Revision Summary

Comprehensive cheat sheet covering Chapters 1–20. For deeper review, see each chapter's own `flashcards.md`.

## 🚀 Core C Concepts Quick-Reference

### 1. Pointers & Memory (Chapter 9)
- `int x = 5;` → Allocates memory for integer.
- `int *p = &x;` → `p` stores the address of `x`.
- `*p = 10;` → Modifies `x` to `10` via dereferencing.
- `p++` → Jumps forward by `sizeof(int)` (usually 4 bytes) — pointer arithmetic is type-scaled.
- `(*n)++` increments the value pointed to; `*n++` increments the pointer itself (precedence trap).

### 2. Recursion (Chapter 10)
- Every recursive function needs a **base case** (stops recursion) and a **recursive case** (progresses toward it).
- No reachable base case → infinite recursion → **stack overflow**.
- Classic examples: Factorial, Fibonacci, GCD (Euclidean), Tower of Hanoi.
- Calls "unwind" in reverse order: the last call to hit the base case is the first to return.

### 3. Arrays & Multidimensional Arrays (Chapters 13, 14)
- `int arr[5];` → Valid indices are `0` through `4`. **No automatic bounds checking.**
- `arr` is equivalent to `&arr[0]`; `arr[i]` ≡ `*(arr+i)`.
- Passing an array to a function always requires passing its size separately.
- 2-D arrays are stored in **row-major order**; `matrix[i]` gives the address of row `i`.
- Passing a 2-D array to a function requires specifying the **column count** in the parameter type.

### 4. Data Types Revisited & Storage Classes (Chapter 11)
- `short`(2B) < `int`(4B) < `long`(4/8B) < `long long`(8B); `unsigned` doubles the positive range.
- Signed integer overflow silently wraps around — no runtime error.
- `auto` (default): garbage until initialized, resets every call.
- `static` (local): initialized once, retains value across calls, defaults to `0`.
- `extern`: declares a variable defined in another file, for cross-file sharing.

### 5. The C Preprocessor (Chapter 12)
- `#define PI 3.14159` (no semicolon!). Purely **textual substitution**, before compilation.
- `#define SQUARE(x) ((x) * (x))` — always parenthesize parameters AND the whole expansion.
- `SQUARE(i++)` evaluates `i++` **twice** — macros can double-evaluate side effects; functions don't.
- Header guards: `#ifndef X_H` / `#define X_H` / ... / `#endif` prevent duplicate inclusion.

### 6. Strings & Multiple Strings (Chapters 15, 16)
- Strings are `char` arrays ending in `'\0'`; allocate at least `strlen+1` bytes.
- `scanf("%s", ...)` stops at whitespace; use `fgets()` for full lines with spaces.
- Never use `==` or `=` on strings/arrays — always use `strcmp()`/`strcpy()`.
- Multiple strings: 2-D `char` array (writable, fixed-width rows) vs. array of `char *` (memory-efficient, often read-only if from literals).

### 7. Structures (Chapter 17)
- **Struct:** Each member gets its own memory (plus possible padding for alignment).
  ```c
  struct Student { char name[50]; int roll; };
  struct Student s1;
  s1.roll = 1; // Dot operator
  ```
- **Pointer to Struct:** Use the arrow operator.
  ```c
  struct Student *ptr = &s1;
  ptr->roll = 2; // Arrow operator
  ```
- Unlike arrays, an entire structure **can** be copied with plain `=`.
- Passing a structure by value copies it wholly; pass a pointer to let a function modify the caller's original.

### 8. Console & File I/O (Chapters 18, 19, 20)
- Formatted I/O (`printf`/`scanf`, `sprintf`/`sscanf`) vs. unformatted (`getchar`/`putchar`/`fgets`/`puts`).
- `gets()` is unsafe (no bounds checking) and removed from C11 — always use `fgets()`.
- Open: `FILE *fp = fopen("file.txt", "r");` (Modes: `r`, `w`, `a` — `"w"` truncates, `"a"` preserves!)
- Always check: `if (fp == NULL) { /* handle error */ }`
- Write: `fprintf(fp, "Data: %d\n", num);` · Read: `while (fscanf(fp, "%d", &num) == 1) { ... }`
- `fread`/`fwrite` for raw binary structures; `fseek(fp, offset, SEEK_SET)` to jump to a specific record.
- Close: `fclose(fp);` — always, to avoid data loss.
- `int main(int argc, char *argv[])` — command-line args are always strings; convert with `atoi`/`atof`.
- `stdin`/`stdout`/`stderr` are automatic; send errors to `stderr` so they survive `stdout` redirection.

---

## 🧠 Common Pitfalls to Avoid in the Exam

1. **`scanf` missing `&`:** `scanf("%d", num);` will crash. It must be `&num`. (Exception: strings read with `%s`, where the array name is already an address.)
2. **Infinite Recursion:** A recursive function without a reachable base case causes a Stack Overflow.
3. **Array Out-of-Bounds:** Accessing `arr[10]` in `int arr[10];` is undefined behavior.
4. **Macro Evaluation:** `SQUARE(2+3)` expands to `2+3*2+3 = 11` if not properly parenthesized as `((x)*(x))`.
5. **`feof()` checked too early:** Use the read function's own return value (`fscanf() == 1`, `fgets() != NULL`) as the loop condition, not `feof()` beforehand.
6. **Comparing strings/structures with `==`:** Use `strcmp()` for strings; whole-structure `==` comparison is not even legal in C — compare member by member if needed.

Review your labs and good luck!
