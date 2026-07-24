# 🃏 Flashcards — Chapter 11: Data Types Revisited

---

**Q: What does the `sizeof` operator return?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The size, in bytes, of a given type or variable.
</details>

---

**Q: What is the typical size of `short`, `int`, `long`, `long long` on a 64-bit GCC system?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `short`=2, `int`=4, `long`=8 (Linux)/4 (Windows), `long long`=8 bytes.
</details>

---

**Q: What happens on signed integer overflow in C?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It silently wraps around (e.g., `INT_MAX + 1` becomes a large negative number) — no runtime error is raised.
</details>

---

**Q: Is plain `char` guaranteed to be signed or unsigned?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Neither — it's implementation-defined. Use `signed char`/`unsigned char` explicitly if the sign matters.
</details>

---

**Q: Which real type should you default to for most computations, `float` or `double`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `double` — better precision (~15-16 significant digits vs ~6-7 for `float`).
</details>

---

**Q: What is the default storage class of a local variable?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `auto`.
</details>

---

**Q: What is the default initial value of an `auto` variable? Of a `static` variable?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `auto`: garbage (uninitialized). `static`: automatically `0` if not explicitly initialized.
</details>

---

**Q: Does a `static` local variable's value persist between function calls?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Yes — it retains its value across calls; only its *scope* stays limited to the block, not its lifetime.
</details>

---

**Q: When does a `static` variable's explicit initializer run?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Only once, the very first time execution reaches that declaration.
</details>

---

**Q: What does `register` request from the compiler, and can it be ignored?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It requests storage in a fast CPU register; the compiler is free to ignore this hint, and modern compilers usually optimize better on their own.
</details>

---

**Q: What is `extern` used for?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Declaring (not defining) a global variable that is actually defined in another source file, enabling sharing across multiple files.
</details>

---

**Q: If a global variable is marked `static`, what changes about its visibility?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It becomes visible only within the current file, instead of being accessible from other files via `extern`.
</details>
