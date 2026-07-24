# 🃏 Flashcards — Chapter 13: Arrays

---

**Q: What is an array?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A fixed-size collection of same-typed elements stored in contiguous memory, accessed via a common name and index.
</details>

---

**Q: What are the valid indices for `int arr[5];`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `0` through `4` (zero-based indexing).
</details>

---

**Q: Does C automatically check array bounds at runtime?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No — out-of-bounds access is undefined behaviour; the programmer is fully responsible.
</details>

---

**Q: What does an array's name "decay" into when used in an expression?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A pointer to its first element (`arr` behaves like `&arr[0]`).
</details>

---

**Q: What is the pointer-notation equivalent of `arr[i]`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `*(arr + i)`.
</details>

---

**Q: Can you write `arr = arr + 1;` for an array `arr`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No — the array name itself is not a reassignable pointer variable, unlike a separate pointer variable.
</details>

---

**Q: Why must array size be passed explicitly to a function receiving the array?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Inside the function, the parameter is just a pointer; `sizeof` on it gives the pointer's size, not the element count, so the size cannot be deduced automatically.
</details>

---

**Q: Are arrays passed to functions by value or by reference in effect?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Effectively by reference — the array decays to a pointer, so modifications inside the function affect the caller's original array.
</details>

---

**Q: How is pointer arithmetic on arrays automatically scaled?**
<details><summary><b>Reveal Answer</b></summary>

**A:** By the size of the pointed-to type — `p + 1` advances by `sizeof(type)` bytes, landing exactly on the next element.
</details>

---

**Q: What is a variable-length array (VLA)?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A C99 feature allowing an array's size to be determined at runtime rather than compile time.
</details>

---

**Q: What risk do large VLAs carry?**
<details><summary><b>Reveal Answer</b></summary>

**A:** They're allocated on the stack, which is limited in size, so a very large VLA can cause a stack overflow.
</details>

---

**Q: What does `int zeros[5] = {0};` initialize the array to?**
<details><summary><b>Reveal Answer</b></summary>

**A:** All 5 elements to `0` — a partial initializer list zero-fills the remaining elements.
</details>
