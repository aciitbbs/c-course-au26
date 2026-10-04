# Supplementary Topic — Quiz: Searching and Sorting

## 📖 Topics Covered: Linear search, binary search (iterative and recursive), selection sort, bubble sort, insertion sort, efficiency and stability

---

## Part A: Multiple Choice Questions (5)

### Q1. Which condition must hold for binary search to work correctly on an array?

A) The array must have an even number of elements
B) The array must be sorted
C) The array must not contain duplicate values
D) The key must be present in the array

<details>
<summary><b>Answer</b></summary>

**B) The array must be sorted**

Binary search decides which half to discard by comparing the key with the middle element. That decision is only valid if every element to the left of `mid` is smaller and every element to the right is larger, i.e. the array is sorted. Duplicates and odd/even sizes are fine, and a missing key is simply reported as "not found".
</details>

---

### Q2. Approximately how many comparisons does binary search need, in the worst case, to search a sorted array of 1,000,000 elements?

A) 20
B) 1,000
C) 500,000
D) 1,000,000

<details>
<summary><b>Answer</b></summary>

**A) 20**

Each comparison halves the remaining range, so the worst case is about `log₂ n` comparisons. Since `2²⁰ = 1,048,576`, about 20 comparisons are enough. Linear search would need up to 1,000,000 (option D).
</details>

---

### Q3. Which of the following sorting algorithms performs **at most `n - 1` swaps** to sort an array of `n` elements?

A) Bubble sort
B) Insertion sort
C) Selection sort
D) All three

<details>
<summary><b>Answer</b></summary>

**C) Selection sort**

Selection sort finds the minimum of the unsorted part and performs one swap per pass, and there are `n - 1` passes. Bubble sort can swap on almost every comparison (up to `n(n-1)/2` swaps for reverse-sorted input), and insertion sort performs a comparable number of shifts.
</details>

---

### Q4. What is the content of the array `{4, 3, 2, 1}` after the **first pass** of bubble sort (ascending order)?

A) `1 2 3 4`
B) `3 4 2 1`
C) `3 2 1 4`
D) `1 4 3 2`

<details>
<summary><b>Answer</b></summary>

**C) `3 2 1 4`**

Pass 1 compares adjacent pairs from left to right: (4,3) swap → `3 4 2 1`; (4,2) swap → `3 2 4 1`; (4,1) swap → `3 2 1 4`. The largest element, 4, has bubbled to the last position. Option D is what selection sort would produce after pass 1.
</details>

---

### Q5. What is the time complexity of insertion sort when the input array is **already sorted**?

A) O(1)
B) O(log n)
C) O(n)
D) O(n²)

<details>
<summary><b>Answer</b></summary>

**C) O(n)**

For each element, the `while` loop makes one comparison, finds that the previous element is not larger, and stops immediately with no shifting. That is `n - 1` comparisons in total, i.e. linear time. This best case is why insertion sort is preferred for nearly sorted data.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Compare linear search and binary search. When would you prefer linear search even though binary search is faster?

<details>
<summary><b>Model Answer</b></summary>

| | Linear Search | Binary Search |
|---|---|---|
| Method | Compare the key with each element from first to last | Compare with the middle element and discard half the range each time |
| Requirement | None: works on unsorted data | Data must be sorted |
| Worst case | `n` comparisons, O(n) | about `log₂ n` comparisons, O(log n) |
| Best case | 1 (key is first) | 1 (key is in the middle) |

**Prefer linear search when:**
- the data is **unsorted** and will only be searched once or twice: sorting first costs O(n²) with the simple sorts, far more than a single O(n) linear search;
- the array is **very small**, so the difference is negligible and linear search is simpler;
- the data is stored in a structure without direct access to the middle element, such as a linked list.
</details>

---

### Q2. Trace binary search for `key = 25` on the array `{3, 7, 11, 15, 19, 23, 27}`. Show `low`, `high` and `mid` at each step and state the result.

<details>
<summary><b>Model Answer</b></summary>

Indices 0–6; `mid = low + (high - low) / 2`.

| Step | `low` | `high` | `mid` | `arr[mid]` | Action |
|---|---|---|---|---|---|
| 1 | 0 | 6 | 3 | 15 | 15 < 25, so `low = 4` |
| 2 | 4 | 6 | 5 | 23 | 23 < 25, so `low = 6` |
| 3 | 6 | 6 | 6 | 27 | 27 > 25, so `high = 5` |
| — | 6 | 5 | — | — | `low > high`: loop ends |

**Result:** 25 is **not found**; the function returns `-1` after 3 comparisons.
</details>

---

### Q3. Show the state of the array `{29, 10, 14, 37, 13}` after each pass of selection sort (ascending order).

<details>
<summary><b>Model Answer</b></summary>

| Pass | Minimum of unsorted part | Swap | Array after pass |
|---|---|---|---|
| Start | — | — | 29 10 14 37 13 |
| 1 | 10 (index 1) | `arr[0]` ↔ `arr[1]` | **10** 29 14 37 13 |
| 2 | 13 (index 4) | `arr[1]` ↔ `arr[4]` | **10 13** 14 37 29 |
| 3 | 14 (index 2) | none needed (already in place) | **10 13 14** 37 29 |
| 4 | 29 (index 4) | `arr[3]` ↔ `arr[4]` | **10 13 14 29 37** |

After `n - 1 = 4` passes the array is sorted. Note that pass 3 still performs all its comparisons even though no element moves: selection sort always makes `n(n-1)/2 = 10` comparisons.
</details>

---

### Q4. Show the state of the array `{9, 5, 1, 4, 3}` after each step of insertion sort (ascending order), and state how many elements are shifted in each step.

<details>
<summary><b>Model Answer</b></summary>

| `i` | `key` | Elements shifted right | Shifts | Array after inserting `key` |
|---|---|---|---|---|
| Start | — | — | — | **9** 5 1 4 3 |
| 1 | 5 | 9 | 1 | **5 9** 1 4 3 |
| 2 | 1 | 9, 5 | 2 | **1 5 9** 4 3 |
| 3 | 4 | 9, 5 | 2 | **1 4 5 9** 3 |
| 4 | 3 | 9, 5, 4 | 3 | **1 3 4 5 9** |

Total shifts: 8. At every step, the bold part on the left is sorted, and each new `key` is placed into its correct position within it.
</details>

---

### Q5. What does it mean for a sorting algorithm to be **stable**? Which of selection, bubble and insertion sort are stable? Show with an example that selection sort is not.

<details>
<summary><b>Model Answer</b></summary>

A sort is **stable** if elements with **equal keys** keep the same relative order in the output as they had in the input. This matters when sorting records: e.g., a list of students already in alphabetical order, re-sorted by marks, should keep students with equal marks in alphabetical order.

- **Bubble sort:** stable (it only swaps adjacent elements when one is *strictly* greater).
- **Insertion sort:** stable (it stops shifting at an element that is *not greater* than the key, so it never jumps over an equal one).
- **Selection sort:** **not stable**, because its long-distance swap can move an element past an equal one.

**Example:** sort the records `(A, 5), (B, 5), (C, 3)` by number using selection sort.

- Pass 1: the minimum is `(C, 3)` at index 2; swap with index 0 → `(C, 3), (B, 5), (A, 5)`.
- Pass 2: the minimum of `(B, 5), (A, 5)` is `(B, 5)` (the first one found with `<`), so no swap → `(C, 3), (B, 5), (A, 5)`.

`A` came before `B` in the input, but `B` comes before `A` in the output, so the original order of the equal keys was not preserved.
</details>
