# 🃏 Flashcards — Chapter 20: More Issues In Input/Output

---

**Q: What does `argc` represent in `main(int argc, char *argv[])`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The number of command-line tokens, including the program's own name.
</details>

---

**Q: What type is every element of `argv`, regardless of what it looks like?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A string (`char *`) — numeric-looking arguments must be converted with `atoi`/`atof` before arithmetic use.
</details>

---

**Q: What does `argv[0]` always contain?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The program's own invocation name/path.
</details>

---

**Q: Why is checking `feof(fp)` before a read a common mistake?**
<details><summary><b>Reveal Answer</b></summary>

**A:** EOF is only detected after a failed read attempt, so checking it beforehand can cause the last item to be processed twice or an extra spurious iteration.
</details>

---

**Q: What should be used as the primary loop-termination check when reading a file?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The read function's own return value (e.g., `fscanf() == 1`, `fgets() != NULL`).
</details>

---

**Q: What are the three standard streams every C program has automatically?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `stdin`, `stdout`, `stderr`.
</details>

---

**Q: Why should error messages go to `stderr` instead of `stdout`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** So they remain visible even if `stdout` is redirected to a file; the two streams can be redirected independently.
</details>

---

**Q: What shell symbol redirects a program's standard output to a file?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `>` (e.g., `./program > output.txt`).
</details>

---

**Q: What shell symbol redirects a program's standard input to come from a file?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `<` (e.g., `./program < input.txt`).
</details>

---

**Q: What is a practical benefit of I/O redirection for testing?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It enables automated testing — feeding fixed input files and capturing output files without manual typing.
</details>

---

**Q: What does `ferror(fp)` report?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Whether a genuine error (not just normal end-of-file) occurred on the stream.
</details>
