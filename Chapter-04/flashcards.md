# 🃏 Flashcards — Chapter 4: More Complex Decision Making

---

**Q: What are the three logical operators in C?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `&&` (AND), `||` (OR), `!` (NOT).
</details>

---

**Q: What does "short-circuit evaluation" mean for `&&`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** If the left operand of `&&` is false, the right operand is never evaluated, since the result is already known to be false.
</details>

---

**Q: What does "short-circuit evaluation" mean for `||`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** If the left operand of `||` is true, the right operand is never evaluated, since the result is already known to be true.
</details>

---

**Q: How can short-circuiting be used to avoid a division-by-zero crash?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Write `if (divisor != 0 && numerator / divisor > x)` — the division is only evaluated once `divisor != 0` is confirmed true.
</details>

---

**Q: What does `!` do to a zero value? To a non-zero value?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `!0` → `1` (true); `!(non-zero)` → `0` (false).
</details>

---

**Q: What is an `else if` ladder, and when does it stop checking conditions?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A chain of `if / else if / else if / ... / else`; it stops at the first condition that evaluates to true and skips the rest.
</details>

---

**Q: Write the syntax of the ternary operator.**
<details><summary><b>Reveal Answer</b></summary>

**A:** `condition ? valueIfTrue : valueIfFalse`
</details>

---

**Q: Is the ternary operator a statement or an expression? Why does that matter?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It's an expression — it produces a value that can be assigned or printed directly, unlike `if-else` which is a statement.
</details>

---

**Q: State De Morgan's law for `!(A && B)`.**
<details><summary><b>Reveal Answer</b></summary>

**A:** `!(A && B)` ≡ `(!A) || (!B)`.
</details>

---

**Q: State De Morgan's law for `!(A || B)`.**
<details><summary><b>Reveal Answer</b></summary>

**A:** `!(A || B)` ≡ `(!A) && (!B)`.
</details>

---

**Q: In precedence terms, which is evaluated first: `&&` or `||`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `&&` has higher precedence than `||`, so it binds tighter (similar to how `*` binds tighter than `+`).
</details>

---

**Q: Where does the ternary operator `?:` fall relative to relational and assignment operators in precedence?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Lower than relational/logical operators, but higher than assignment (`=`).
</details>

---

**Q: Why should compound logical conditions generally be parenthesized even when not strictly required?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It removes ambiguity for the reader and prevents precedence-related bugs, even though the compiler would evaluate it identically without them.
</details>
