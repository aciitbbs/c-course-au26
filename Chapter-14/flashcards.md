# 🃏 Flashcards — Chapter 14: Multidimensional Arrays

---

**Q: In what order does C store 2-D array elements in memory?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Row-major order — the entire first row, then the entire second row, and so on, contiguously.
</details>

---

**Q: What does `matrix[i]` represent for a 2-D array?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The address of row `i` (equivalent to `&matrix[i][0]`), behaving like a 1-D array/pointer.
</details>

---

**Q: What is `matrix[i][j]` equivalent to in pointer notation?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `*(*(matrix + i) + j)`.
</details>

---

**Q: When passing a 2-D array to a function, which dimension is mandatory in the parameter type?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The number of columns — needed to compute each row's memory stride. Rows can be a separate parameter.
</details>

---

**Q: What's the difference between `int (*p)[3]` and `int *p[3]`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `(*p)[3]` is a pointer to a 3-element array (steps a full row at a time); `*p[3]` is an array of 3 separate int pointers.
</details>

---

**Q: What formula gives the address of `matrix[i][j]` given base address, COLS, and element size?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `base + (i * COLS + j) * sizeof(element)`.
</details>

---

**Q: What condition must hold for matrix multiplication A (r1×c1) by B (r2×c2) to be valid?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `c1 == r2` (columns of A must equal rows of B); the result is `r1 × c2`.
</details>

---

**Q: How many nested loops does the standard matrix multiplication algorithm use, and why?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Three — one for result rows, one for result columns, and one to accumulate the dot-product sum over the shared dimension.
</details>

---

**Q: What is a practical use case for an "array of pointers" over a true 2-D array?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Jagged arrays (rows of different lengths) or arrays of strings, where each pointer refers to an independently-sized block.
</details>

---

**Q: How many nested loops are needed to traverse a 3-D array fully?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Three — one per dimension.
</details>
