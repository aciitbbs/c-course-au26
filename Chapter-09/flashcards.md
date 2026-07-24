# 🃏 Flashcards — Chapter 9: Pointers

---

**Q: What is a pointer?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A variable that stores the memory address of another variable.
</details>

---

**Q: What does the `&` operator do?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Returns the memory address of its operand (address-of operator).
</details>

---

**Q: What does the `*` operator do when used on a pointer in an expression?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Dereferences the pointer — accesses the value stored at the address it holds.
</details>

---

**Q: In `int *p;`, does `*` mean "dereference" here?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No — in a declaration, `*` is part of the type, meaning "p is a pointer to int," not an operation.
</details>

---

**Q: Why does plain call-by-value fail to implement a `swap()` function?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The function only receives copies of the arguments; changes to those copies never affect the caller's original variables.
</details>

---

**Q: How does call by reference let a function modify the caller's variable?**
<details><summary><b>Reveal Answer</b></summary>

**A:** By passing the variable's address (a pointer); dereferencing that pointer inside the function accesses/modifies the original variable directly.
</details>

---

**Q: Why does `scanf("%d", &x)` need the `&`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `scanf` must write into `x`; passing its address lets `scanf` dereference and store the value directly at `x`'s memory location (call by reference).
</details>

---

**Q: What is a "wild" pointer, and why is dereferencing one dangerous?**
<details><summary><b>Reveal Answer</b></summary>

**A:** An uninitialized pointer holding a garbage address; dereferencing it is undefined behaviour and can crash the program or corrupt memory.
</details>

---

**Q: What does `*n++` actually do, compared to `(*n)++`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `*n++` parses as `*(n++)` — increments the pointer itself. `(*n)++` increments the value it points to. They are NOT the same.
</details>

---

**Q: On a 64-bit system, does `sizeof(int*)` differ from `sizeof(double*)`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No — all pointer variables have the same fixed size (e.g., 8 bytes) regardless of the pointed-to type, since a pointer only stores an address.
</details>

---

**Q: Why is call by reference essential for a function that needs to "return" two or more results?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A single `return` can hand back only one value; passing multiple output pointers lets a function update several caller variables directly instead.
</details>

---

**Q: What value should an uninitialized pointer be set to, as a safe default?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `NULL` — and it should be checked (`if (p != NULL)`) before being dereferenced.
</details>
