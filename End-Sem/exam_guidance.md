# End-Semester Exam Guidance

## 📅 Exam Overview

- **Total Marks:** 50
- **Duration:** 3 Hours
- **Format:** Pen and Paper (Theory, Tracing, & Code Writing)
- **Dates:** November 23 – December 2, 2026
- **Syllabus Covered:** [Chapter 1](../Chapter-01/) through [Chapter 20](../Chapter-20/) (Comprehensive). Emphasis will be on post-mid-sem topics: Recursion, Multidimensional Arrays, Data Types Revisited, the Preprocessor, Strings, Structures, and File Handling — but Mid-Sem topics (Chapters 1–9, 13) can still appear, especially Pointers and Arrays.

---

## 📚 Syllabus Breakdown (Post-Mid-Sem Emphasis)

1. **[Chapter 10 — Recursion](../Chapter-10/lecture.md)**
   - Base case vs. recursive step, the call stack, tracing recursive functions (Factorial, Fibonacci, GCD, Tower of Hanoi).

2. **[Chapter 14 — Multidimensional Arrays](../Chapter-14/lecture.md)**
   - Row-major memory layout, `matrix[i][j]` ≡ `*(*(matrix+i)+j)`, passing 2-D arrays to functions (column count mandatory).
   - Matrix addition, multiplication, and transpose.

3. **[Chapter 11 — Data Types Revisited](../Chapter-11/lecture.md)**
   - `short`/`long`/`unsigned` variants, `sizeof`, integer overflow.
   - Storage classes: `auto`, `register`, `static`, `extern`.

4. **[Chapter 12 — The C Preprocessor](../Chapter-12/lecture.md)**
   - `#define` macros (object-like and function-like), macro pitfalls (parenthesization, multiple evaluation), `#include`, conditional compilation, header guards.

5. **[Chapter 15](../Chapter-15/lecture.md) & [Chapter 16 — Strings](../Chapter-16/lecture.md)**
   - The null terminator `'\0'`, `<string.h>` functions (`strlen`, `strcpy`, `strcmp`, `strcat`).
   - `fgets` vs `scanf("%s")`; storing multiple strings (2-D array vs. array of pointers); sorting/searching strings with `strcmp`.

6. **[Chapter 17 — Structures](../Chapter-17/lecture.md)**
   - Grouping heterogeneous data; the dot `.` operator (value) vs. the arrow `->` operator (pointer).
   - Structure copying with `=`, array of structures, passing structures by value vs. by pointer.

7. **[Chapter 18](../Chapter-18/lecture.md) & [Chapter 19 — I/O and Files](../Chapter-19/lecture.md)**
   - Field width/precision in `printf`; `sprintf`/`sscanf`; `getchar`/`putchar`; the danger of `gets()`.
   - The `FILE*` pointer, `fopen` modes (`"r"`, `"w"`, `"a"`), `fgetc`/`fputc`, `fgets`/`fputs`, `fprintf`/`fscanf`, `fread`/`fwrite`, `fseek`.

8. **[Chapter 20 — More Issues in I/O](../Chapter-20/lecture.md)**
   - `argc`/`argv` command-line arguments, `feof`/`ferror`, standard streams (`stdin`/`stdout`/`stderr`), I/O redirection.

### Also still in scope (from Mid-Sem, may reappear)

9. **[Chapter 9 — Pointers](../Chapter-09/lecture.md)** and **[Chapter 13 — Arrays](../Chapter-13/lecture.md)** — address-of/dereference, call by reference, array-pointer equivalence, passing arrays/pointers to functions.

---

## 📝 Tips for the Exam

1. **Trace Code on Paper:** Many questions will ask "What is the output of this code?". Draw boxes for variables, cross them out when they change, and track pointers and recursive calls carefully.
2. **Watch the Bounds:** C doesn't check array bounds. If a question shows a loop going from `0` to `<= 5` for an array of size 5, it's an out-of-bounds error!
3. **Strings Need Space:** Remember that a string like `"Hi"` requires 3 bytes of memory, not 2, because of `'\0'`. Never compare strings with `==`; always use `strcmp`.
4. **Don't Forget Base Cases:** If asked to write a recursive function, the absolute first thing you should write is the base case.
5. **Structures:** Remember `.` for structure variables, `->` for structure pointers. Whole structures can be copied with `=`; arrays cannot.
6. **Files:** Always check `fopen`'s return value for `NULL` before using it. Remember `"w"` truncates existing content; `"a"` preserves it.
7. **Macros:** Always expect a question testing `#define SQUARE(x) x*x` without parentheses — know exactly why it fails for `SQUARE(2+3)`.
8. **Write Clean Code:** If asked to write a program, use proper indentation (even on paper) and add brief comments to explain your logic.

Good luck!
