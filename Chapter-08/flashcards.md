# 🃏 Flashcards — Chapter 8: Functions

---

**Q: What are the three parts of a function's general syntax?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Return type, function name with parameter list, and the body containing statements (with an optional `return`).
</details>

---

**Q: What are the four categories of functions based on arguments/return value?**
<details><summary><b>Reveal Answer</b></summary>

**A:** (1) No args, no return; (2) args, no return; (3) no args, a return; (4) args and a return.
</details>

---

**Q: What is a function prototype, and why is it needed?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A declaration of a function's name, return type, and parameter types (ending in `;`, no body); needed so the compiler can validate calls made before the function's actual definition appears.
</details>

---

**Q: What happens to code written immediately after a `return` statement in the same execution path?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It becomes unreachable — `return` exits the function immediately, so any following code in that path never executes.
</details>

---

**Q: What return type should a function use if it doesn't return any value?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `void`.
</details>

---

**Q: What header declares `sqrt()` and `pow()`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `<math.h>` (sometimes requiring the `-lm` linker flag).
</details>

---

**Q: Why is calling a function before declaring/defining it dangerous in old C, and what do modern compilers do instead?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Old C assumed it returns `int` without type-checking — dangerous. Modern compilers (C99+) raise an error/strong warning instead.
</details>

---

**Q: What benefit does breaking a program into functions give for debugging?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Each function can be tested independently, making it much easier to isolate the source of a bug than searching one large `main()`.
</details>

---

**Q: In a function call, how are arguments matched to parameters?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Positionally — the first argument maps to the first parameter, the second to the second, and so on.
</details>

---

**Q: Is the order of evaluation of multiple arguments in a function call guaranteed by the C standard?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No — it is unspecified; avoid code that relies on a particular evaluation order of arguments with side effects.
</details>

---

**Q: How many values can a single `return` statement send back directly?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Exactly one value of the declared return type (multiple "returned" values require pointers/structures, covered later).
</details>

---

**Q: What does `main()` conventionally return, and what do the values mean?**
<details><summary><b>Reveal Answer</b></summary>

**A:** An `int`; `0` conventionally means success, non-zero means an error occurred.
</details>
