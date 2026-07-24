# 🃏 Flashcards — Chapter 19: File Input/Output

---

**Q: What are the three essential steps for working with a file in C?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Open (`fopen`) → Read/Write → Close (`fclose`).
</details>

---

**Q: What does `fopen` return if it fails to open a file?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `NULL` — always check for this before using the file pointer.
</details>

---

**Q: What happens to existing content when you `fopen(file, "w")` an existing file?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It is truncated (erased) immediately upon opening.
</details>

---

**Q: Which mode preserves existing content and adds new writes at the end?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `"a"` (append).
</details>

---

**Q: What is the risk of forgetting to call `fclose()`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Buffered data may never actually reach disk (data loss), and file-descriptor resources may leak.
</details>

---

**Q: Which function pair is used for reading/writing raw binary structures?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `fread()` / `fwrite()`.
</details>

---

**Q: Which function repositions the file pointer to a specific byte offset?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `fseek(fp, offset, whence)`, with `whence` being `SEEK_SET`, `SEEK_CUR`, or `SEEK_END`.
</details>

---

**Q: What does `SEEK_SET` mean in `fseek`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The offset is measured from the beginning of the file.
</details>

---

**Q: Why is binary mode preferred over text mode for saving raw structures?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Binary mode performs no newline/character translation, preserving the structure's exact byte layout.
</details>

---

**Q: What value does `fgetc()` return at the end of a file?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `EOF` (End Of File marker).
</details>

---

**Q: How can you tell if `fread()` successfully read the requested number of items?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Compare its return value (items actually read) to the count you requested — fewer means end-of-file or an error occurred.
</details>
