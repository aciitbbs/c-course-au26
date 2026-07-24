# 🃏 Flashcards — Chapter 5: Loop Control Instruction

---

**Q: What three ingredients does every loop need?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Initialization, condition, and an update step that eventually makes the condition false.
</details>

---

**Q: Why is `while` called "entry-controlled"?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Because the condition is tested *before* each iteration (including the first), so the body may run zero times.
</details>

---

**Q: What bug does `while (i <= 5); { ... }` (semicolon after the condition) create?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The semicolon makes the loop body an empty statement; since nothing updates `i`, it becomes an infinite loop producing no output.
</details>

---

**Q: What is the #1 cause of accidental infinite loops?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Forgetting to update the loop's control variable inside the loop body.
</details>

---

**Q: Difference between `i++` and `++i`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `i++` (post-increment) returns the old value then increments; `++i` (pre-increment) increments first then returns the new value.
</details>

---

**Q: What does `x = i++;` do if `i` starts at 5?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `x` gets `5` (the old value), then `i` becomes `6`.
</details>

---

**Q: What does `x = ++i;` do if `i` starts at 5?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `i` becomes `6` first, then `x` gets `6`.
</details>

---

**Q: What is an "off-by-one" error?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A loop that runs one time too many or too few, usually from confusing `<` with `<=` (or vice versa) in the loop condition.
</details>

---

**Q: How do you extract the last digit of an integer `n`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `n % 10`.
</details>

---

**Q: How do you remove the last digit of an integer `n`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `n = n / 10;` (integer division discards the last digit).
</details>

---

**Q: In the prime-check loop, why test only up to `i * i <= n`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** If `n` has no factor ≤ √n, it cannot have any factor > √n either, so testing beyond √n is redundant — this avoids needing `<math.h>`.
</details>

---

**Q: What compound assignment operator is shorthand for `sum = sum + i;`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `sum += i;`
</details>

---

**Q: Is the order of evaluation of `i++` and `i` guaranteed inside the same `printf()` argument list, e.g. `printf("%d %d", i++, i)`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No — this is undefined behaviour in C; never rely on evaluation order of side effects within the same statement/argument list.
</details>
