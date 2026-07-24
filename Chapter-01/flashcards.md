# 🃏 Flashcards — Chapter 1: Getting Started

Quick-revision cards. Try to answer before revealing!

---

**Q: Who created the C language, and in what year?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Dennis Ritchie, at Bell Telephone Laboratories, in **1972**.
</details>

---

**Q: What are the three main stages of turning a `.c` file into a running program?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Preprocessing → Compilation → Linking (source → expanded source → object code → executable).
</details>

---

**Q: What does `#include <stdio.h>` do?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Tells the preprocessor to insert the contents of the Standard Input/Output header, which declares functions like `printf()` and `scanf()`.
</details>

---

**Q: Why must every C program have a `main()` function?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Because `main()` is the designated entry point — the operating system starts executing the program there, regardless of where it is written in the file.
</details>

---

**Q: How many keywords does standard C have, and can they be used as variable names?**
<details><summary><b>Reveal Answer</b></summary>

**A:** 32 keywords (e.g., `int`, `if`, `while`, `return`). They cannot be used as variable or function names.
</details>

---

**Q: What value is internally stored for the character constant `'A'`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Its ASCII code, 65 (an integer). Character constants are stored as integers.
</details>

---

**Q: Name three rules for constructing a valid C variable name.**
<details><summary><b>Reveal Answer</b></summary>

**A:** (1) Must start with a letter or underscore; (2) may contain only letters/digits/underscores; (3) cannot be a keyword; (4) is case-sensitive; (5) no blanks or special symbols other than `_`.
</details>

---

**Q: What is the difference between declaration and initialization?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Declaration (`int x;`) only reserves memory; initialization (`int x = 5;`) additionally assigns a starting value at the same time.
</details>

---

**Q: Why is `&` required before variables in `scanf()`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `&` is the address-of operator; `scanf()` needs to know the *memory address* of the variable to store the typed-in value there.
</details>

---

**Q: What is a "garbage value"?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The unpredictable leftover bit pattern in an uninitialized variable's memory location; C does not auto-initialize local variables to zero.
</details>

---

**Q: What is the typical size (in bytes) of `char`, `int`, `float`, and `double` on a modern system?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `char` = 1, `int` = 4, `float` = 4, `double` = 8.
</details>

---

**Q: What escape sequence produces a newline? A tab?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `\n` for newline, `\t` for a horizontal tab.
</details>

---

**Q: Difference between an integer constant, a real constant, and a character constant?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Integer: whole number, no decimal point (`123`). Real: has a decimal point or exponent (`3.14`, `6.02e23`). Character: single character in single quotes (`'A'`).
</details>

---

**Q: What does `return 0;` at the end of `main()` signify?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It tells the operating system the program terminated successfully. A non-zero return conventionally signals an error condition.
</details>

---

**Q: Is C case-sensitive? Give an example showing why this matters.**
<details><summary><b>Reveal Answer</b></summary>

**A:** Yes. `total`, `Total`, and `TOTAL` are three completely different identifiers to the compiler.
</details>

---

**Q: What is the correct `gcc` command to compile `hello.c` into an executable named `hello`, with warnings enabled?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `gcc -Wall -Wextra hello.c -o hello`
</details>
