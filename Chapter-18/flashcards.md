# 🃏 Flashcards — Chapter 18: Console Input/Output

---

**Q: What's the difference between formatted and unformatted I/O functions?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Formatted (`printf`/`scanf`) use format strings and type conversion; unformatted (`getchar`/`putchar`/`fgets`/`puts`) work with raw characters/lines, no conversion.
</details>

---

**Q: What does `%8.2f` mean in a `printf` format string?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Minimum field width 8, precision 2 (2 digits after the decimal point).
</details>

---

**Q: What does `printf("%-10s", name)` do differently from `%10s`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `%-10s` left-aligns within the 10-character field; `%10s` right-aligns (the default).
</details>

---

**Q: What do `sprintf`/`sscanf` operate on, instead of the console?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A string buffer — `sprintf` writes formatted text into a string; `sscanf` parses formatted text out of a string.
</details>

---

**Q: Why is `gets()` dangerous?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It has no way to limit input length, risking a buffer overflow; it was removed from C11.
</details>

---

**Q: What is the safe replacement for `gets()`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `fgets(buffer, size, stdin)`, which takes an explicit maximum size.
</details>

---

**Q: What does `scanf`'s return value tell you?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The number of items successfully matched and assigned — useful for validating input.
</details>

---

**Q: Why does a plain `%c` in `scanf` sometimes read a leftover newline unexpectedly?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A prior `%d`/`%f` read leaves the trailing `'\n'` in the buffer, and plain `%c` (unlike `%d`) doesn't skip leading whitespace.
</details>

---

**Q: How do you fix the leftover-newline problem before reading a character?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Add a leading space in the format string: `scanf(" %c", &ch);`.
</details>

---

**Q: What do `getchar()` and `putchar()` operate on?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A single character at a time — reading from stdin and writing to stdout respectively.
</details>

---

**Q: Does `fgets()` include the newline character it reads, if there's room?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Yes — unlike `gets()`, which discards it.
</details>
