# 🃏 Flashcards — Chapter 10: Recursion

---

**Q: What two components must every recursive function have?**
<details><summary><b>Reveal Answer</b></summary>

**A:** A base case (stops recursion) and a recursive case (calls itself on a smaller sub-problem).
</details>

---

**Q: What happens if a recursive function has no reachable base case?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It recurses indefinitely, eventually causing a stack overflow (the recursive analogue of an infinite loop).
</details>

---

**Q: What data structure keeps track of pending recursive calls?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The call stack — each call gets its own stack frame with its own local variables/parameters.
</details>

---

**Q: In `factorial(n) = n * factorial(n-1)`, what is the base case?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `n == 0` (or `n == 1`), returning `1`.
</details>

---

**Q: Why is naive recursive Fibonacci inefficient?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It recomputes the same sub-values repeatedly, leading to exponential time complexity as `n` grows.
</details>

---

**Q: What guarantees the recursive Euclidean GCD function terminates?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The second argument (`a % b`) strictly decreases with each call and cannot go below 0, so it must eventually hit the base case `b == 0`.
</details>

---

**Q: In Tower of Hanoi, why does the function call itself twice per level?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Once to move `n-1` disks out of the way to the auxiliary peg, and once again to move those `n-1` disks from the auxiliary peg onto the destination, after the largest disk is moved directly.
</details>

---

**Q: Can every recursive algorithm be rewritten as an iterative (loop-based) one?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Yes, in principle — recursion and iteration are equally powerful; the choice is about clarity, memory, and performance.
</details>

---

**Q: What real-world resource is consumed excessively by very deep or infinite recursion?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Stack memory — each nested call adds a new stack frame, and too many cause a stack overflow crash.
</details>

---

**Q: In which order do recursive calls "return" their results — same order as calls were made, or reverse?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Reverse order — the last call to reach the base case is the first to return, and results propagate back up ("unwind") from there.
</details>

---

**Q: What is the recursive case for `sumOfN(n) = n + sumOfN(n-1)`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Adding `n` to the sum of all natural numbers up to `n-1`, i.e., reducing the problem to a smaller version of itself.
</details>
