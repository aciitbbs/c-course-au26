# 🃏 Flashcards — Chapter 3: Decision Control Instruction

---

**Q: What is the difference between `if` and `if-else`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `if` executes a block only when true and does nothing otherwise; `if-else` picks exactly one of two alternative blocks depending on the condition.
</details>

---

**Q: What's the danger of writing `if (x = 5)` instead of `if (x == 5)`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `x = 5` is an assignment; it sets `x` to 5 and the expression evaluates to `5` (non-zero → always true), silently overwriting `x` and always taking the "true" branch.
</details>

---

**Q: Why must multiple statements after an `if` be wrapped in `{ }`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Without braces, `if`/`else` controls only the single statement immediately following it; later statements execute unconditionally regardless of indentation.
</details>

---

**Q: What does a semicolon right after `if (condition)` cause?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It creates an empty statement as the "then" branch, so the actual code on the next line runs unconditionally — a subtle bug.
</details>

---

**Q: What is the "dangling else" problem?**
<details><summary><b>Reveal Answer</b></summary>

**A:** In nested `if`s without braces, an `else` always binds to the nearest preceding unmatched `if`, which may not be the `if` the programmer intended.
</details>

---

**Q: How do you fix a dangling-else ambiguity?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Add explicit braces `{ }` around the inner `if` so it becomes "matched," freeing the `else` to correctly bind to the outer `if`.
</details>

---

**Q: List the six relational/equality operators in C.**
<details><summary><b>Reveal Answer</b></summary>

**A:** `<`, `<=`, `>`, `>=`, `==`, `!=`.
</details>

---

**Q: In an `if-else-if` grading ladder, why does order of conditions matter?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The first true condition (top to bottom) wins and the rest are skipped, so ranges must be ordered correctly (e.g., check `>= 90` before `>= 75`) or later, narrower conditions will never be reached.
</details>

---

**Q: What condition determines if three side lengths a, b, c can form a valid triangle?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `a+b > c && b+c > a && a+c > b` — the sum of any two sides must exceed the third.
</details>

---

**Q: Does C have a Boolean type used by `if`? What values count as true/false?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Classic C uses `int` for conditions: `0` is false, and any non-zero value is true.
</details>

---

**Q: Why is `if (year % 400 == 0)` needed inside the leap-year nested-if, even after checking `% 100 == 0`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Century years (divisible by 100) are leap years only if also divisible by 400 (e.g., 2000 is, but 1900 is not).
</details>

---

**Q: Does indentation affect which statements an `if` controls in C?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No. Indentation is purely cosmetic; only braces `{ }` (or the single next statement) determine what is controlled.
</details>
