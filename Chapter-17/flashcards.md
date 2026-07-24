# 🃏 Flashcards — Chapter 17: Structures

---

**Q: What does `struct` allow you to do that a plain array cannot?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Group related members of *different* data types into a single named unit.
</details>

---

**Q: Which operator accesses a member of a structure variable directly?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The dot operator (`.`), e.g., `s1.rollNumber`.
</details>

---

**Q: Which operator accesses a member through a structure pointer?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The arrow operator (`->`), e.g., `ptr->rollNumber`, equivalent to `(*ptr).rollNumber`.
</details>

---

**Q: Can you copy an entire structure with `=`, unlike arrays?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Yes — `s2 = s1;` copies every member of `s1` into `s2` in one statement.
</details>

---

**Q: If a function receives a structure by value and modifies it, does the caller see the change?**
<details><summary><b>Reveal Answer</b></summary>

**A:** No — the function only modifies its own local copy; pass a pointer to modify the original.
</details>

---

**Q: What is a nested structure?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A structure that has another structure as one of its members, accessed via chained dot operators.
</details>

---

**Q: Why can `sizeof(struct)` be larger than the sum of its members' sizes?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The compiler may insert padding bytes between members for memory alignment/CPU efficiency.
</details>

---

**Q: What does `typedef struct {...} Student;` let you do?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Declare structure variables as `Student s1;` without repeating the `struct` keyword each time.
</details>

---

**Q: What is an "array of structures" used for?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Storing many records of the same composite "shape" (e.g., a list of students), just like an array stores many values of the same simple type.
</details>

---

**Q: How do you access the nested member `year` inside `s1.dob` where `dob` is a `struct Date`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `s1.dob.year` — chain the dot operators.
</details>
