# 🃏 Flashcards — Supplementary Topic: Dynamic Memory Allocation

---

**Q: What is dynamic memory allocation?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Requesting memory from the heap at run time (with `malloc()`, `calloc()` or `realloc()`) and releasing it explicitly with `free()`.
</details>

---

**Q: Which header declares `malloc()`, `calloc()`, `realloc()` and `free()`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `<stdlib.h>`.
</details>

---

**Q: Where do local variables live, and where does `malloc()`'d memory live?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Local variables live on the **stack** and are released automatically when the function returns; `malloc()`'d memory lives on the **heap** and lasts until `free()` is called.
</details>

---

**Q: What does `malloc()` return, and what type is it?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The base address of the allocated block as a `void *`, or `NULL` if allocation fails.
</details>

---

**Q: Correct way to allocate an array of `n` floats with `malloc()`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `float *p = (float *) malloc(n * sizeof(float));` followed by a `NULL` check.
</details>

---

**Q: What values does a freshly `malloc()`'d block contain?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Garbage: `malloc()` does not initialize memory.
</details>

---

**Q: How does `calloc()` differ from `malloc()`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It takes two arguments (`count`, `size`) and sets every byte of the block to zero.
</details>

---

**Q: What does `realloc(p, new_size)` do?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Resizes the block at `p`, preserving its contents; it may move the block and return a new address, or return `NULL` (leaving the old block intact) on failure.
</details>

---

**Q: Why assign `realloc()`'s result to a temporary pointer first?**
<details><summary><b>Reveal Answer</b></summary>

**A:** If it fails and returns `NULL`, writing straight into `p` would lose the only pointer to the original block, causing a memory leak.
</details>

---

**Q: What is a memory leak?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Heap memory that is never freed and can no longer be reached because every pointer to it has been lost or overwritten.
</details>

---

**Q: What is a dangling pointer?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A pointer that still holds the address of memory that has been freed (or has gone out of scope); using it is undefined behaviour.
</details>

---

**Q: Why write `p = NULL;` after `free(p);`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It prevents use-after-free and double-free bugs; `free(NULL)` is harmless.
</details>

---

**Q: How many bytes must be allocated to copy a string `s`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `strlen(s) + 1`: the extra byte holds the terminating `'\0'`.
</details>

---

**Q: Can a function safely return a pointer to its own local array? To a `malloc()`'d block?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Local array: **no**, it is destroyed when the function returns. `malloc()`'d block: **yes**, it survives until freed, and the caller becomes responsible for freeing it.
</details>

---

**Q: How do you allocate a single `struct Student` dynamically and set its `roll` member?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `struct Student *s = malloc(sizeof(struct Student)); s->roll = 101;`
</details>

---

**Q: In what order do you free a dynamic 2-D array built from row pointers?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Free each row `m[i]` first, then free the array of row pointers `m`.
</details>
