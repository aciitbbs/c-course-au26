# 🃏 Flashcards — Chapter 12: The C Preprocessor

---

**Q: When does the preprocessor run relative to the compiler?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Before compilation — it processes `#` directives and produces a fully expanded translation unit that the compiler then reads.
</details>

---

**Q: Does the preprocessor understand C syntax or types?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No — it performs purely textual substitution with no knowledge of C grammar or types.
</details>

---

**Q: Why must macro parameters and the whole macro body be parenthesized?**
<details><summary><b>Reveal Answer</b></summary>

**A:** To avoid operator-precedence bugs when the macro is expanded textually into a larger surrounding expression.
</details>

---

**Q: What does `#define SQUARE(x) x * x` expand `SQUARE(2+3)` into, and what's wrong with it?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `2 + 3 * 2 + 3` = 11, not the intended 25 — missing parentheses caused the bug.
</details>

---

**Q: What danger exists in `SQUARE(i++)` if `SQUARE` uses its argument twice?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `i++` gets textually duplicated and evaluated twice, incrementing `i` twice — undefined/unexpected behaviour, unlike a real function where arguments evaluate once.
</details>

---

**Q: Difference between `#include <stdio.h>` and `#include "myheader.h"`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Angle brackets search system/standard directories; quotes search the local project directory first.
</details>

---

**Q: What is a header guard, and what pattern implements it?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A pattern (`#ifndef X_H / #define X_H / ... / #endif`) that prevents a header's contents from being processed more than once if included multiple times.
</details>

---

**Q: What is the main advantage of conditional compilation (`#if DEBUG`) over a runtime `if (DEBUG)` check for debug logging?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Conditional compilation removes the code entirely at compile time when disabled — zero runtime cost, versus a runtime check that still executes (and evaluates) the condition every time.
</details>

---

**Q: What does `#undef MACRO` do?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Removes a previously defined macro, allowing it to be safely redefined afterward.
</details>

---

**Q: Why do macros lack type safety compared to functions?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Because they are pure text substitution with no compiler type-checking of "arguments" — anything textually valid gets substituted, errors surface only after expansion (if at all).
</details>

---

**Q: What does `gcc -E program.c` do, and why is it useful?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Outputs the fully preprocessed source (after macro expansion/`#include`), useful for debugging tricky macro-expansion issues.
</details>

---

**Q: In modern C style, what is often preferred over `#define` for simple typed constants?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `const` variables (or `enum` for integers) — they have real types, respect scope, and are visible to debuggers.
</details>
