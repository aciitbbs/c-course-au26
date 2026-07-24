# 🃏 Flashcards — Chapter 15: Strings

---

**Q: What character terminates a C string?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The null character `'\0'` (ASCII value 0).
</details>

---

**Q: How many bytes must be allocated for an n-character string?**
<details><summary><b>Reveal Answer</b></summary>

**A:** At least `n + 1` bytes — the extra byte holds the `'\0'` terminator.
</details>

---

**Q: Why doesn't `scanf("%s", name)` need `&` before `name`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `name` (an array) already decays to the address of its first character.
</details>

---

**Q: Why can't `scanf("%s", ...)` read a full sentence with spaces?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It stops reading at the first whitespace character; use `fgets()` for multi-word input.
</details>

---

**Q: What does `strlen("Hello")` return?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `5` — it counts visible characters only, excluding the `'\0'` terminator.
</details>

---

**Q: Why is `strcpy()` considered risky?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It performs no bounds checking; copying a string longer than the destination buffer causes a buffer overflow.
</details>

---

**Q: What must be true about `dest`'s capacity before calling `strcat(dest, src)`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It must have enough remaining space for both its existing content and all of `src`, plus the terminator.
</details>

---

**Q: What does `strcmp(s1, s2)` return when the strings are equal?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `0`.
</details>

---

**Q: Why is `if (str1 == str2)` wrong for comparing string content?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `==` compares pointer addresses, not character contents; use `strcmp(str1, str2) == 0` instead.
</details>

---

**Q: Does `fgets()` include the trailing newline in the buffer?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Yes, if there was room — it's often manually stripped afterward (e.g., with `strcspn`).
</details>

---

**Q: What is the standard idiom for manually traversing a string with an index?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `for (i = 0; str[i] != '\0'; i++) { ... }`
</details>

---

**Q: What header must be included to use `strlen`, `strcpy`, `strcat`, `strcmp`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `<string.h>`
</details>
