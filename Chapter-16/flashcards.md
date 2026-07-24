# 🃏 Flashcards — Chapter 16: Handling Multiple Strings

---

**Q: What are the two common ways to store multiple strings in C?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A 2-D `char` array (fixed-width rows), or an array of `char *` pointers (variable-width, memory-efficient).
</details>

---

**Q: What's the main drawback of a 2-D `char` array for storing strings of varying length?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Every row must be as wide as the longest string, wasting memory for shorter strings.
</details>

---

**Q: Why is modifying `names[0][0]` dangerous when `names` is an array of pointers initialized from string literals?**
<details><summary><b>Reveal Answer</b></summary>

**A:** String literals are typically stored in read-only memory; writing to them is undefined behaviour and may crash the program.
</details>

---

**Q: Which storage approach guarantees the strings are safely writable?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The 2-D `char` array approach — each row is genuine, independently writable memory.
</details>

---

**Q: How must you compare two strings when sorting a list of strings?**
<details><summary><b>Reveal Answer</b></summary>

**A:** With `strcmp()`, never `<`, `>`, or `==` (those compare addresses, not content).
</details>

---

**Q: Why must `strcpy()` be used to "swap" strings during sorting instead of `=`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `char` array rows cannot be assigned directly with `=`; their content must be copied character-by-character via `strcpy`.
</details>

---

**Q: What does `strcmp(a, b) > 0` mean?**
<details><summary><b>Reveal Answer</b></summary>

**A:** String `a` comes lexicographically *after* string `b`.
</details>

---

**Q: In a linear search over an array of strings, what stops the search early?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Finding a `strcmp(arr[i], key) == 0` match — the loop can `break`/`return` immediately.
</details>

---

**Q: When is the array-of-pointers approach preferred over a 2-D array?**
<details><summary><b>Reveal Answer</b></summary>

**A:** When memory efficiency matters more than mutability — e.g., a large, fixed list of read-only strings of very different lengths.
</details>
