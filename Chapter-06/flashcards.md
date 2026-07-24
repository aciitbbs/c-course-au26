# 🃏 Flashcards — Chapter 6: More Complex Repetitions

---

**Q: What three things does a `for` loop header combine?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Initialization, condition, and update — all in one place: `for (init; condition; update)`.
</details>

---

**Q: Is `for` entry-controlled or exit-controlled?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Entry-controlled — same as `while`; the body can execute zero times.
</details>

---

**Q: For nested loops with outer running `m` times and inner running `n` times, how many total times does the inner statement execute?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `m × n` times.
</details>

---

**Q: What does `break` do inside a loop?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Immediately terminates the nearest enclosing loop (or `switch`), transferring control to the statement right after it.
</details>

---

**Q: What does `continue` do inside a loop?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Skips the rest of the current iteration's body and jumps to the next iteration (condition re-check, and for `for` loops, the update runs first).
</details>

---

**Q: Does `break` exit outer loops too, when used inside a nested inner loop?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No — `break` only exits the innermost loop that directly contains it.
</details>

---

**Q: Why is `do-while` exit-controlled, and what guarantee does that give?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Its condition is checked *after* the body runs, guaranteeing the body executes at least once.
</details>

---

**Q: Why is `do-while` the natural choice for menu-driven programs?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The menu must be shown at least once before any user choice exists to check against.
</details>

---

**Q: What must never be forgotten after a `do-while` loop's condition?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The terminating semicolon: `} while (condition);`
</details>

---

**Q: Give two equivalent ways to write an intentional infinite loop in C.**
<details><summary><b>Reveal Answer</b></summary>

**A:** `for (;;) { ... }` and `while (1) { ... }` — both need an internal `break` to actually terminate.
</details>

---

**Q: In `for (i = 0, j = 10; i < j; i++, j--)`, what C feature allows two variables to be initialized/updated?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The comma operator, which lets multiple expressions appear in the init/update slots of a `for` header.
</details>

---

**Q: How does `for` relate to `while`? Is one strictly more powerful?**
<details><summary><b>Reveal Answer</b></summary>

**A:** They are functionally equivalent — any `for` loop can be rewritten as a `while` loop with explicit init/update statements, and vice versa. `for` is just more compact syntax for counter-driven loops.
</details>
