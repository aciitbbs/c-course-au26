# Supplementary Topic — Lecture: Searching and Sorting

## 📖 References

- *Let Us C*, 19th Edition — Chapter 13: "Arrays" (summary on selection, bubble and insertion sort; Figure 13.3 and Exercise [B](d) on insertion sort)
- Chapter 13 lecture notes in this repository (linear search and bubble sort worked programs); Chapter 16 (sorting an array of strings)

> **Prerequisites:** Arrays and passing arrays to functions (Chapter 13), Recursion (Chapter 10), Structures (Chapter 17).

---

## S.1 Introduction

**Searching** means finding whether a given value (the **key**) is present in a collection and, if so, where. **Sorting** means arranging the elements of a collection in ascending or descending order.

They are studied together because they are closely linked: a **sorted** array can be searched far faster (binary search) than an unsorted one (linear search).

### S.1.1 Measuring Efficiency

To compare algorithms we count the **number of basic operations** (usually comparisons) as a function of the input size `n`, and look at how that count grows.

| Notation | Name | Meaning for `n = 1000` |
|---|---|---|
| `O(1)` | Constant | A fixed number of steps, regardless of `n` |
| `O(log n)` | Logarithmic | About 10 steps |
| `O(n)` | Linear | About 1,000 steps |
| `O(n²)` | Quadratic | About 1,000,000 steps |

We usually discuss three cases:

- **Best case:** the input that needs the fewest operations.
- **Worst case:** the input that needs the most operations. This is the guarantee we can rely on.
- **Average case:** the expected number of operations over typical inputs.

Two more terms for sorting:

- **In-place:** the algorithm needs only a few extra variables (such as `temp`), not a second array. All three sorts in this lecture are in-place.
- **Stable:** elements with equal keys keep their original relative order. This matters when sorting records (for example, students already sorted by name, now sorted by marks).

## S.2 Linear Search

**Idea:** compare the key with every element in turn, from first to last. Stop as soon as it matches. If the end is reached without a match, the key is absent.

Linear search works on **any** array, sorted or not.

```c
int linearSearch(int arr[], int n, int key)
{
    int i;
    for (i = 0; i < n; i++)
    {
        if (arr[i] == key)
            return i;      /* found: return its index */
    }
    return -1;             /* not found */
}
```

### S.2.1 Trace

Searching for `key = 23` in `{14, 7, 23, 9, 31}`:

| `i` | `arr[i]` | `arr[i] == 23`? |
|---|---|---|
| 0 | 14 | No |
| 1 | 7 | No |
| 2 | 23 | **Yes**: return 2 |

### S.2.2 Analysis

| Case | When it happens | Comparisons |
|---|---|---|
| Best | Key is the first element | 1 |
| Worst | Key is the last element, or absent | `n` |
| Average | Key equally likely to be anywhere | about `n / 2` |

Linear search is **O(n)**.

## S.3 Binary Search

**Precondition: the array must already be sorted** (we assume ascending order).

**Idea:** look at the **middle** element.

- If it equals the key, we are done.
- If the key is **larger**, it can only be in the **right half**, so discard the left half.
- If the key is **smaller**, it can only be in the **left half**, so discard the right half.

Each comparison halves the part of the array still to be searched, so very few comparisons are needed.

### S.3.1 Iterative Binary Search

```c
int binarySearch(int arr[], int n, int key)
{
    int low = 0, high = n - 1, mid;

    while (low <= high)
    {
        mid = low + (high - low) / 2;

        if (arr[mid] == key)
            return mid;              /* found */
        else if (arr[mid] < key)
            low = mid + 1;           /* search the right half */
        else
            high = mid - 1;          /* search the left half */
    }
    return -1;                       /* low > high: not found */
}
```

> **Why `low + (high - low) / 2` and not `(low + high) / 2`?** Both give the same middle index, but for very large arrays `low + high` can exceed the largest value an `int` can hold (integer overflow). The first form never does. Either form is accepted in exams, but the safer one is a good habit.

### S.3.2 Trace: Key Found

Array (indices 0–9): `{2, 5, 8, 12, 16, 23, 38, 56, 72, 91}`, `key = 23`

| Step | `low` | `high` | `mid` | `arr[mid]` | Action |
|---|---|---|---|---|---|
| 1 | 0 | 9 | 4 | 16 | 16 < 23, so `low = 5` |
| 2 | 5 | 9 | 7 | 56 | 56 > 23, so `high = 6` |
| 3 | 5 | 6 | 5 | 23 | **Found at index 5** |

Only 3 comparisons for 10 elements. Linear search would have needed 6.

### S.3.3 Trace: Key Absent

Same array, `key = 40`:

| Step | `low` | `high` | `mid` | `arr[mid]` | Action |
|---|---|---|---|---|---|
| 1 | 0 | 9 | 4 | 16 | 16 < 40, so `low = 5` |
| 2 | 5 | 9 | 7 | 56 | 56 > 40, so `high = 6` |
| 3 | 5 | 6 | 5 | 23 | 23 < 40, so `low = 6` |
| 4 | 6 | 6 | 6 | 38 | 38 < 40, so `low = 7` |
| — | 7 | 6 | — | — | `low > high`: **not found**, return -1 |

### S.3.4 Recursive Binary Search

Binary search is naturally recursive (Chapter 10): each call searches a smaller sub-array.

```c
int binarySearchRec(int arr[], int low, int high, int key)
{
    int mid;

    if (low > high)
        return -1;                                     /* base case: empty range */

    mid = low + (high - low) / 2;

    if (arr[mid] == key)
        return mid;                                    /* base case: found */
    else if (arr[mid] < key)
        return binarySearchRec(arr, mid + 1, high, key);
    else
        return binarySearchRec(arr, low, mid - 1, key);
}

/* first call: binarySearchRec(arr, 0, n - 1, key); */
```

### S.3.5 Analysis

After `k` comparisons, at most `n / 2^k` elements remain. The search ends when this falls to 1, i.e. after about **log₂ n** comparisons. Binary search is **O(log n)**.

| `n` | Linear search (worst) | Binary search (worst) |
|---|---|---|
| 10 | 10 | 4 |
| 1,000 | 1,000 | 10 |
| 1,000,000 | 1,000,000 | 20 |

### S.3.6 Linear vs Binary Search

| | Linear Search | Binary Search |
|---|---|---|
| Requires sorted data? | No | **Yes** |
| Worst-case comparisons | `n` | about `log₂ n` |
| Complexity | O(n) | O(log n) |
| Works on a linked list? | Yes | Not efficiently (no direct access to the middle) |
| Best for | Small or unsorted data, or a one-off search | Large sorted data searched many times |

## S.4 Selection Sort

**Idea:** find the **smallest** element in the whole array and swap it into position 0. Then find the smallest of the remaining elements and swap it into position 1, and so on. After pass `i`, positions `0` to `i` hold the final sorted values.

*Let Us C* summary: "compare 0th element with all others, 1st with others, etc."

```c
void selectionSort(int arr[], int n)
{
    int i, j, minIndex, temp;

    for (i = 0; i < n - 1; i++)
    {
        minIndex = i;
        for (j = i + 1; j < n; j++)
        {
            if (arr[j] < arr[minIndex])
                minIndex = j;
        }

        /* swap the smallest remaining element into position i */
        temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
}
```

### S.4.1 Trace

Sorting `{64, 25, 12, 22, 11}`:

| Pass | Smallest in unsorted part | Swap | Array after pass |
|---|---|---|---|
| Start | — | — | 64 25 12 22 11 |
| 1 | 11 (index 4) | `arr[0]` ↔ `arr[4]` | **11** 25 12 22 64 |
| 2 | 12 (index 2) | `arr[1]` ↔ `arr[2]` | **11 12** 25 22 64 |
| 3 | 22 (index 3) | `arr[2]` ↔ `arr[3]` | **11 12 22** 25 64 |
| 4 | 25 (index 3) | none needed | **11 12 22 25 64** |

### S.4.2 Analysis

- Comparisons: always `(n-1) + (n-2) + ... + 1 = n(n-1)/2`, whatever the input. Best, average and worst cases are all **O(n²)**.
- Swaps: at most `n - 1`, the fewest of the three sorts. Useful when writing to memory is expensive.
- **Not stable** in the form above: the long-distance swap can jump an element over an equal one.

## S.5 Bubble Sort

**Idea:** compare **adjacent** pairs and swap them if they are in the wrong order. After one full pass, the largest element has "bubbled up" to the last position. Repeat for the remaining elements.

The basic version appears as Program 3 in the Chapter 13 lecture. The improved version below adds a **`swapped` flag**: if a complete pass makes no swaps, the array is already sorted and the algorithm stops early.

```c
void bubbleSort(int arr[], int n)
{
    int i, j, temp, swapped;

    for (i = 0; i < n - 1; i++)
    {
        swapped = 0;
        for (j = 0; j < n - 1 - i; j++)
        {
            if (arr[j] > arr[j + 1])
            {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = 1;
            }
        }
        if (swapped == 0)
            break;        /* no swaps in this pass: already sorted */
    }
}
```

The inner loop runs only up to `n - 1 - i` because the last `i` elements are already in their final places.

### S.5.1 Trace

Sorting `{5, 1, 4, 2, 8}`:

**Pass 1:**

| Compare | Swap? | Array |
|---|---|---|
| 5, 1 | Yes | 1 5 4 2 8 |
| 5, 4 | Yes | 1 4 5 2 8 |
| 5, 2 | Yes | 1 4 2 5 8 |
| 5, 8 | No | 1 4 2 5 **8** |

**Pass 2:** compare (1,4) no, (4,2) yes, (4,5) no → `1 2 4 5 8`

**Pass 3:** compare (1,2) no, (2,4) no → no swaps, so **stop early**.

Result: `1 2 4 5 8`, after 3 passes instead of 4.

### S.5.2 Analysis

| Case | When it happens | Comparisons |
|---|---|---|
| Best | Already sorted (with the `swapped` flag) | `n - 1` (one pass), **O(n)** |
| Worst | Reverse sorted | `n(n-1)/2`, **O(n²)** |
| Average | Random order | **O(n²)** |

Bubble sort is **stable**: it only swaps adjacent elements when one is strictly greater, so equal elements never pass each other.

## S.6 Insertion Sort

**Idea:** this is how most people sort a hand of playing cards. Treat the first element as a sorted part of length 1. Take the next element (the **key**), shift every larger element in the sorted part one place to the right, and insert the key into the gap. Repeat until every element has been inserted.

*Let Us C* summary: "Insert each successive element at appropriate position amongst elements before it."

```c
void insertionSort(int arr[], int n)
{
    int i, j, key;

    for (i = 1; i < n; i++)
    {
        key = arr[i];
        j = i - 1;

        /* shift elements of the sorted part that are greater than key */
        while (j >= 0 && arr[j] > key)
        {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;     /* drop key into the gap */
    }
}
```

> **Order of the `while` condition matters.** `j >= 0` must come first: when `j` becomes `-1`, short-circuit evaluation (Chapter 4) stops before `arr[-1]` is read.

### S.6.1 Trace

Sorting `{44, 33, 55, 22, 11}` (the example from *Let Us C* Figure 13.3). The sorted part is shown in bold.

| `i` | `key` | Elements shifted right | Array after inserting `key` |
|---|---|---|---|
| Start | — | — | **44** 33 55 22 11 |
| 1 | 33 | 44 | **33 44** 55 22 11 |
| 2 | 55 | none | **33 44 55** 22 11 |
| 3 | 22 | 55, 44, 33 | **22 33 44 55** 11 |
| 4 | 11 | 55, 44, 33, 22 | **11 22 33 44 55** |

### S.6.2 Analysis

| Case | When it happens | Comparisons |
|---|---|---|
| Best | Already sorted: the `while` loop never shifts | `n - 1`, **O(n)** |
| Worst | Reverse sorted: every key moves to the front | `n(n-1)/2`, **O(n²)** |
| Average | Random order | **O(n²)** |

Insertion sort is **stable** (it uses `>` not `>=`, so it never moves a key past an equal element) and is very fast on small or **nearly sorted** arrays. That is why real-world library sorts often switch to insertion sort for small pieces.

## S.7 Comparing the Three Sorts

| | Selection Sort | Bubble Sort | Insertion Sort |
|---|---|---|---|
| Idea | Select the minimum, swap into place | Swap adjacent out-of-order pairs | Insert each element into the sorted part |
| Best case | O(n²) | O(n) (with `swapped` flag) | O(n) |
| Average case | O(n²) | O(n²) | O(n²) |
| Worst case | O(n²) | O(n²) | O(n²) |
| Number of swaps/moves | At most `n - 1` swaps (fewest) | Many | Many shifts |
| Stable? | No | Yes | Yes |
| In-place? | Yes | Yes | Yes |
| Adapts to nearly sorted data? | No | Yes | Yes (best of the three) |

> All three are **O(n²)** in the worst case and are only practical for small arrays (a few thousand elements). Faster O(n log n) algorithms such as merge sort and quick sort are covered in a Data Structures course. The C standard library also provides `qsort()` in `<stdlib.h>`.

### S.7.1 Sorting in Descending Order

Reverse the comparison:

| Sort | Ascending | Descending |
|---|---|---|
| Selection | `arr[j] < arr[minIndex]` | `arr[j] > arr[maxIndex]` |
| Bubble | `arr[j] > arr[j + 1]` | `arr[j] < arr[j + 1]` |
| Insertion | `arr[j] > key` | `arr[j] < key` |

## S.8 Worked Programs

### Program 1: Sort, Then Binary Search

Binary search needs sorted data, so a common pattern is to sort once and then search as many times as needed.

```c
#include <stdio.h>

void insertionSort(int arr[], int n)
{
    int i, j, key;
    for (i = 1; i < n; i++)
    {
        key = arr[i];
        j = i - 1;
        while (j >= 0 && arr[j] > key)
        {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}

int binarySearch(int arr[], int n, int key)
{
    int low = 0, high = n - 1, mid;
    while (low <= high)
    {
        mid = low + (high - low) / 2;
        if (arr[mid] == key)
            return mid;
        else if (arr[mid] < key)
            low = mid + 1;
        else
            high = mid - 1;
    }
    return -1;
}

int main()
{
    int arr[8] = {42, 7, 19, 73, 4, 56, 31, 88};
    int n = 8, i, key, pos;

    insertionSort(arr, n);

    printf("Sorted array: ");
    for (i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");

    printf("Enter a number to search: ");
    scanf("%d", &key);

    pos = binarySearch(arr, n, key);
    if (pos != -1)
        printf("%d found at index %d of the sorted array\n", key, pos);
    else
        printf("%d not found\n", key);

    return 0;
}
```

**Sample runs:**
```
Sorted array: 4 7 19 31 42 56 73 88
Enter a number to search: 56
56 found at index 5 of the sorted array
```
```
Sorted array: 4 7 19 31 42 56 73 88
Enter a number to search: 50
50 not found
```

### Program 2: Counting Comparisons

This program runs all three sorts on copies of the same data and counts the comparisons each one makes, showing how the input order affects each algorithm.

```c
#include <stdio.h>

int selectionCount(int a[], int n)
{
    int i, j, m, t, c = 0;
    for (i = 0; i < n - 1; i++)
    {
        m = i;
        for (j = i + 1; j < n; j++)
        {
            c++;
            if (a[j] < a[m])
                m = j;
        }
        t = a[i]; a[i] = a[m]; a[m] = t;
    }
    return c;
}

int bubbleCount(int a[], int n)
{
    int i, j, t, swapped, c = 0;
    for (i = 0; i < n - 1; i++)
    {
        swapped = 0;
        for (j = 0; j < n - 1 - i; j++)
        {
            c++;
            if (a[j] > a[j + 1])
            {
                t = a[j]; a[j] = a[j + 1]; a[j + 1] = t;
                swapped = 1;
            }
        }
        if (!swapped)
            break;
    }
    return c;
}

int insertionCount(int a[], int n)
{
    int i, j, key, c = 0;
    for (i = 1; i < n; i++)
    {
        key = a[i];
        j = i - 1;
        while (j >= 0)
        {
            c++;
            if (a[j] <= key)
                break;
            a[j + 1] = a[j];
            j--;
        }
        a[j + 1] = key;
    }
    return c;
}

void copy(int src[], int dst[], int n)
{
    int i;
    for (i = 0; i < n; i++)
        dst[i] = src[i];
}

void compare(char label[], int data[], int n)
{
    int a[10];
    printf("%-15s", label);
    copy(data, a, n); printf("%10d", selectionCount(a, n));
    copy(data, a, n); printf("%8d", bubbleCount(a, n));
    copy(data, a, n); printf("%11d\n", insertionCount(a, n));
}

int main()
{
    int sorted[10]   = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int reversed[10] = {10, 9, 8, 7, 6, 5, 4, 3, 2, 1};
    int random[10]   = {7, 2, 9, 4, 10, 1, 8, 3, 6, 5};

    printf("%-15s%10s%8s%11s\n", "Input", "Selection", "Bubble", "Insertion");
    compare("Already sorted", sorted, 10);
    compare("Reverse sorted", reversed, 10);
    compare("Random", random, 10);
    return 0;
}
```

**Output:**
```
Input           Selection  Bubble  Insertion
Already sorted         45       9          9
Reverse sorted         45      45         45
Random                 45      39         31
```

Selection sort always makes `n(n-1)/2 = 45` comparisons. Bubble sort (with the flag) and insertion sort drop to `n - 1 = 9` on sorted input, which is their O(n) best case.

### Program 3: Sorting an Array of Structures

Sorting records works exactly like sorting integers: compare one **member** (the sort key), and swap **whole structures** (Chapter 17). Here, selection sort orders students by marks, highest first.

```c
#include <stdio.h>

struct Student
{
    char name[20];
    int marks;
};

int main()
{
    struct Student s[5] = {
        {"Ishaan", 72}, {"Tara", 91}, {"Vikram", 65}, {"Nisha", 88}, {"Rohan", 79}
    };
    struct Student temp;
    int n = 5, i, j, maxIndex;

    for (i = 0; i < n - 1; i++)
    {
        maxIndex = i;
        for (j = i + 1; j < n; j++)
            if (s[j].marks > s[maxIndex].marks)
                maxIndex = j;

        temp = s[i];
        s[i] = s[maxIndex];
        s[maxIndex] = temp;
    }

    printf("Merit list:\n");
    for (i = 0; i < n; i++)
        printf("%d. %-8s %d\n", i + 1, s[i].name, s[i].marks);

    return 0;
}
```

**Output:**
```
Merit list:
1. Tara     91
2. Nisha    88
3. Rohan    79
4. Ishaan   72
5. Vikram   65
```

To sort strings (names) instead, compare with `strcmp()` as shown in the Chapter 16 lecture (§16.4).

## S.9 Common Pitfalls

| Pitfall | Example | Fix |
|---|---|---|
| Binary search on unsorted data | Calling `binarySearch()` on `{42, 7, 19, ...}` | Sort first, or use linear search |
| Wrong loop condition in binary search | `while (low < high)` misses the case `low == high` | Use `while (low <= high)` |
| Not moving past `mid` | `low = mid;` or `high = mid;` | Use `mid + 1` and `mid - 1`, or the loop may never end |
| Returning "not found" too early in linear search | `else return -1;` inside the loop | Return -1 only **after** the loop finishes |
| Bubble sort inner loop overruns | `for (j = 0; j < n; j++)` reads `arr[j + 1] = arr[n]` | Use `j < n - 1 - i` |
| Insertion sort reads `arr[-1]` | `while (arr[j] > key && j >= 0)` | Test `j >= 0` **first** |
| Losing a value while swapping | `arr[i] = arr[j]; arr[j] = arr[i];` | Use a `temp` variable |
| Selection sort swapping inside the inner loop | Swapping every time a smaller element is seen | Record `minIndex`; swap once per pass |

## S.10 Key Takeaways

1. **Linear search** checks every element in turn: O(n), works on any array.
2. **Binary search** repeatedly halves a **sorted** array: O(log n); about 20 comparisons suffice for a million elements.
3. **Selection sort** selects the minimum and swaps it into place: always O(n²) comparisons, but at most `n - 1` swaps; not stable.
4. **Bubble sort** swaps adjacent out-of-order pairs; with a `swapped` flag it stops early and is O(n) on sorted input; stable.
5. **Insertion sort** inserts each element into the sorted part on its left; O(n) on sorted input and the best of the three for nearly sorted data; stable.
6. All three simple sorts are O(n²) in the worst case and sort in place; to sort descending, reverse the comparison.
