# 🃏 Flashcards — Chapter 2: C Instructions

---

**Q: What are the three broad types of C instructions?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Type declaration instructions, arithmetic instructions, and control instructions.
</details>

---

**Q: What does `17 / 5` evaluate to in C, and why?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `3`. Both operands are `int`, so `/` performs integer division and truncates (discards) the fractional part `.4`.
</details>

---

**Q: What does `17 % 5` evaluate to?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `2` (the remainder of `17 / 5`). `%` is only valid for integer operands.
</details>

---

**Q: What is "mixed-mode arithmetic"?**
<details><summary><b>Reveal Answer</b></summary>

**A:** An expression combining operands of different types (e.g., `int` and `float`); C automatically promotes the lower type to the higher type before the operation.
</details>

---

**Q: What's wrong with `float avg = total / count;` when both are `int`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The division is performed as integer division (truncated) *before* the result is stored in `avg`, so precision is already lost. Cast an operand to `float` before dividing: `(float) total / count`.
</details>

---

**Q: Why does `(float)(a / b)` fail to give a precise result when `a` and `b` are both `int`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The division `a / b` happens first (as integer division, truncating), and only afterwards is the already-truncated result cast to `float`. Cast an operand, not the whole expression: `(float) a / b`.
</details>

---

**Q: What is the precedence order of `()`, `*`, `/`, `%`, `+`, `-`, `=`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `()` highest, then `*`/`/`/`%`, then `+`/`-`, then `=` lowest.
</details>

---

**Q: What is "associativity" and why does it matter for `20 / 2 * 5`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Associativity decides evaluation order among same-precedence operators. `*` and `/` are left-to-right associative, so `20 / 2 * 5` = `(20/2)*5` = `50`, not `20/(2*5)`.
</details>

---

**Q: Why is `=` called "right-to-left associative"? Give an example.**
<details><summary><b>Reveal Answer</b></summary>

**A:** In a chain like `x = y = z = 5;`, the rightmost assignment (`z = 5`) happens first, and the result propagates leftward to `y`, then `x`.
</details>

---

**Q: When converting `float` to `int` (e.g., `int i = 7.9;`), is the value rounded or truncated?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Truncated — `i` becomes `7`, not `8`. C discards the fractional part; it does not round.
</details>

---

**Q: What is the difference between implicit and explicit type conversion?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Implicit conversion happens automatically per C's promotion rules; explicit conversion (casting) is manually written by the programmer as `(type) expression`.
</details>

---

**Q: Can the modulus operator `%` be used with `float` operands?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No. `%` is defined only for integer types in standard C arithmetic.
</details>

---

**Q: What is the correct way to compute a percentage `marksObtained/maxMarks * 100` avoiding integer-division loss?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `((float) marksObtained / maxMarks) * 100;` — cast one operand before the division.
</details>

---

**Q: In `int a = 5 + 3 * 2;`, what is `a`, and why?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `11`. `*` (precedence higher than `+`) evaluates first: `3*2=6`, then `5+6=11`.
</details>
