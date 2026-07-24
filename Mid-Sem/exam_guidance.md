# Mid-Semester Exam Guidance

## 📅 Exam Overview

- **Total Marks:** 30
- **Duration:** 2 Hours
- **Format:** Pen and Paper (Theory & Code Writing)
- **Dates:** September 16–24, 2026
- **Syllabus Covered:** [Chapter 1](../Chapter-01/) through [Chapter 9](../Chapter-09/), plus [Chapter 13](../Chapter-13/) (1-D Arrays only — Multidimensional Arrays in Chapter 14 are **NOT** in scope for the Mid-Sem).

---

## 📚 Syllabus Breakdown

1. **[Chapter 1 — Getting Started](../Chapter-01/lecture.md)**
   - Compilation model (Source → Preprocessor → Compiler → Linker → Executable).
   - `#include <stdio.h>`, `main()` structure, constants, variables, keywords.

2. **[Chapter 2 — C Instructions](../Chapter-02/lecture.md)**
   - Arithmetic operators (`+ - * / %`), integer vs. real vs. mixed-mode arithmetic.
   - Implicit and explicit type conversion (casting), operator precedence & associativity.

3. **[Chapter 3 — Decision Control Instruction](../Chapter-03/lecture.md)**
   - `if`, `if-else`, nested `if-else`, the dangling-`else` problem.

4. **[Chapter 4 — More Complex Decision Making](../Chapter-04/lecture.md)**
   - Logical operators (`&&`, `||`, `!`) and short-circuit evaluation.
   - `else-if` ladder, the conditional (ternary) operator `?:`.

5. **[Chapter 5 — Loop Control Instruction](../Chapter-05/lecture.md)**
   - The `while` loop (entry-controlled), increment/decrement operators.

6. **[Chapter 6 — More Complex Repetitions](../Chapter-06/lecture.md)**
   - `for` loop, nested loops, `break`, `continue`, `do-while` (exit-controlled).

7. **[Chapter 7 — Case Control Instruction](../Chapter-07/lecture.md)**
   - `switch-case`, fall-through behaviour, `goto` (brief).

8. **[Chapter 8 — Functions](../Chapter-08/lecture.md)**
   - Function declaration/prototype, definition, call; categories of functions; return values.

9. **[Chapter 9 — Pointers](../Chapter-09/lecture.md)** *(High Priority)*
   - `&` and `*` operators, call by value vs. call by reference, the `swap()` function.

10. **[Chapter 13 — Arrays](../Chapter-13/lecture.md)** *(1-D arrays only)*
    - Declaration, initialization, traversal, bounds (no automatic checking!).
    - Array-pointer equivalence (`arr[i]` ≡ `*(arr+i)`), passing arrays to functions.

---

## 📝 Question Paper Pattern (Expected)

The 30 marks will likely be distributed as follows:

1. **Section A: Objective / Short Answer (10 Marks)**
   - Predict the output of short C snippets (including pointer/array tracing).
   - Identify syntax or logical errors in code.
   - Conceptual questions (e.g., "What is the difference between `while` and `do-while`?", "Why does `scanf` need `&`?").

2. **Section B: Code Writing (10 Marks)**
   - Write a complete C program to solve a specific problem (e.g., check prime number, print a pattern, array search/sort, a `swap()` function using pointers).
   - Focus on correct logic, proper syntax, and edge cases.

3. **Section C: Tracing / Debugging (10 Marks)**
   - Trace the execution of a nested loop, a pointer-manipulation snippet, or a simple array algorithm.
   - Rewrite an `else-if` ladder as a `switch-case`.
   - Explain short-circuit behaviour or pointer/array equivalence in a given expression.

---

## 💡 Tips for Success

1. **Mind the Semicolons:** A missing semicolon can change the logic (e.g., `while(condition);`). Look closely at the code snippets in the output questions.
2. **Integer Division:** Remember that `5 / 2` evaluates to `2`, not `2.5`.
3. **`=` vs `==`:** In an `if` condition, `if (a = 5)` assigns 5 to `a` and evaluates to true. `if (a == 5)` compares them. This is a classic trick question!
4. **Short-Circuiting:** If evaluating `A && B` and `A` is false, `B` is *never* executed. Watch out for `++` or `--` operations hidden inside `B`.
5. **Pointers:** Always distinguish `&x` (address of `x`) from `*p` (value pointed to by `p`). Remember `(*n)++` increments the *value*, while `*n++` increments the *pointer*.
6. **Arrays:** C never checks array bounds — dry-run loop conditions (`<` vs `<=`) very carefully. Remember `arr` decays to `&arr[0]`.
7. **Dry Run on Paper:** For loops, pointers, and array traversal, create a small table for variables (`i`, `j`, `*p`, `arr[i]`) and update them line-by-line as you mentally execute the code.

---

*Use the `revision_summary.md` file for a quick glance at the most critical syntax and concepts.*
