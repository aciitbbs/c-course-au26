# 🃏 Flashcards — Supplementary Topic: Searching and Sorting

---

**Q: How does linear search work?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It compares the key with each element from first to last, returning the index on a match or -1 if the end is reached.
</details>

---

**Q: Best-case and worst-case comparisons for linear search on `n` elements?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Best: 1 (key is first). Worst: `n` (key is last or absent). Complexity O(n).
</details>

---

**Q: What is the precondition for binary search?**
<details><summary><b>Reveal Answer</b></summary>

**A:** The array must be sorted.
</details>

---

**Q: How does binary search work?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It compares the key with the middle element; if smaller, it searches the left half (`high = mid - 1`), if larger the right half (`low = mid + 1`), until found or `low > high`.
</details>

---

**Q: What is the complexity of binary search, and how many comparisons for 1,000,000 elements?**
<details><summary><b>Reveal Answer</b></summary>

**A:** O(log n); about 20 comparisons.
</details>

---

**Q: Why is `mid = low + (high - low) / 2` preferred over `(low + high) / 2`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `low + high` can overflow an `int` for very large arrays; the first form cannot.
</details>

---

**Q: What is the loop condition for iterative binary search?**
<details><summary><b>Reveal Answer</b></summary>

**A:** `while (low <= high)`: using `<` would miss the case where one element remains.
</details>

---

**Q: How does selection sort work?**
<details><summary><b>Reveal Answer</b></summary>

**A:** In each pass it finds the minimum of the unsorted part and swaps it into the first unsorted position.
</details>

---

**Q: How many comparisons and swaps does selection sort make?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Always `n(n-1)/2` comparisons (O(n²) in every case), but at most `n - 1` swaps.
</details>

---

**Q: How does bubble sort work?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It repeatedly compares adjacent pairs and swaps them if out of order; after each pass the largest remaining element reaches its final place at the end.
</details>

---

**Q: What does the `swapped` flag add to bubble sort?**
<details><summary><b>Reveal Answer</b></summary>

**A:** If a full pass makes no swaps, the array is sorted and the sort stops early, giving an O(n) best case on sorted input.
</details>

---

**Q: How does insertion sort work?**
<details><summary><b>Reveal Answer</b></summary>

**A:** It takes each element (the key) in turn, shifts larger elements of the sorted part one place right, and inserts the key into the gap.
</details>

---

**Q: Why must insertion sort test `j >= 0` before `arr[j] > key`?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Short-circuit `&&` then stops at `j == -1` before reading the invalid element `arr[-1]`.
</details>

---

**Q: Which simple sort is best for nearly sorted data?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Insertion sort: it does very little shifting and approaches O(n).
</details>

---

**Q: What is a stable sort? Which of the three simple sorts are stable?**
<details><summary><b>Reveal Answer</b></summary>

**A:** One that keeps equal elements in their original relative order. Bubble and insertion sort are stable; selection sort is not.
</details>

---

**Q: Worst-case complexity of selection, bubble and insertion sort?**
<details><summary><b>Reveal Answer</b></summary>

**A:** O(n²) for all three.
</details>

---

**Q: How do you change any of these sorts to descending order?**
<details><summary><b>Reveal Answer</b></summary>

**A:** Reverse the comparison operator (e.g., `arr[j] > arr[j + 1]` becomes `arr[j] < arr[j + 1]` in bubble sort).
</details>
