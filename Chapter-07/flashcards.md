# 🃏 Flashcards — Chapter 7: Case Control Instruction

---

**Q: What data types can control a `switch` statement in standard C?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Integral types only — `int` and `char` (not `float`/`double`, not strings).
</details>

---

**Q: What does "fall-through" mean in a `switch`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Once a matching case is found, execution continues into the following case's code too, unless a `break` stops it.
</details>

---

**Q: What is the purpose of `break` inside a `switch` case?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It stops execution from falling through into the next case, exiting the `switch` immediately.
</details>

---

**Q: What happens if no `case` matches and there is no `default`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Nothing inside the `switch` executes; control simply continues after it — no error occurs.
</details>

---

**Q: When is fall-through used deliberately (a good thing)?**
<details><summary><b>Reveal Answer</b></summary>

**A:** When stacking multiple case labels without statements between them, so they all share the same code block (e.g., grouping vowels or upper/lower-case letters).
</details>

---

**Q: Can `switch` test a range like `case 1 to 10`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Not in standard, portable C — `switch` only tests exact equality against constants. Use `if-else` for ranges.
</details>

---

**Q: When should you prefer `switch` over `if-else-if`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** When one integral variable is compared against many discrete known constant values (e.g., menu choices).
</details>

---

**Q: What does `goto` do?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Performs an unconditional jump to a labelled statement anywhere in the same function.
</details>

---

**Q: Why is `goto` discouraged in modern C?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Overuse leads to "spaghetti code" that is hard to read, trace, and maintain, since it bypasses structured control flow.
</details>

---

**Q: Name one accepted, legitimate use case for `goto` in modern C.**
<details><summary><b>Reveal Answer</b></summary>

**A:** Centralized error/cleanup handling — jumping to one cleanup label near the end of a function from multiple failure points.
</details>

---

**Q: Where is `default` conventionally placed in a `switch`, and is it mandatory?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Conventionally last, though it may appear anywhere; it is optional, not mandatory.
</details>

---

**Q: Can `case` labels be duplicated in the same `switch`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No — all case values in one `switch` must be distinct constants; duplicates cause a compile-time error.
</details>
