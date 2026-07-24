# Chapter 16 — Quiz: Handling Multiple Strings

## 📖 Topics Covered: 2-D char arrays vs array of pointers to strings, mutability, sorting/searching strings

---

## Part A: Multiple Choice Questions (5)

### Q1. What is the primary memory-efficiency drawback of storing strings using `char names[5][20];`?

A) It cannot store more than 5 strings
B) Every row reserves 20 bytes regardless of the actual string's length, wasting space for shorter strings
C) It requires `<string.h>`
D) It cannot be initialized with string literals

<details>
<summary><b>Answer</b></summary>

**B) Every row reserves 20 bytes regardless of the actual string's length, wasting space for shorter strings**

Since each row must be wide enough for the longest anticipated string, shorter strings in other rows still consume the full fixed row width, wasting unused memory.
</details>

---

### Q2. What is the danger of writing `names[0][0] = 'X';` when `names` was declared as `char *names[3] = {"Alice", "Bob", "Carol"};`?

A) There is no danger; this is always safe
B) It attempts to modify a string literal, which is undefined behaviour
C) It causes a compilation error
D) It only changes the pointer, not the string content

<details>
<summary><b>Answer</b></summary>

**B) It attempts to modify a string literal, which is undefined behaviour**

`names[0]` points to the string literal `"Alice"`, which is typically stored in read-only memory. Attempting to modify it is undefined behaviour and may crash the program on some systems.
</details>

---

### Q3. Which comparison correctly determines whether two strings `s1` and `s2` (stored as `char` arrays) hold the same content?

A) `if (s1 == s2)`
B) `if (s1 = s2)`
C) `if (strcmp(s1, s2) == 0)`
D) `if (s1.equals(s2))`

<details>
<summary><b>Answer</b></summary>

**C) `if (strcmp(s1, s2) == 0)`**

`strcmp` returns `0` exactly when the two strings' contents are identical. Options A and B compare/assign addresses, and option D is not valid C syntax (that is Java-style method-call syntax).
</details>

---

### Q4. When sorting an array of strings (2-D `char` array) using bubble sort, why must `strcpy()` be used for the "swap" step instead of a simple `temp = arr[j];`?

A) `strcpy` is faster than assignment
B) `char` arrays (rows) cannot be assigned directly with `=`; their contents must be copied element-by-element via `strcpy`
C) `temp = arr[j];` would work fine, `strcpy` is just an alternative style
D) `strcpy` is required by the C standard for all string operations

<details>
<summary><b>Answer</b></summary>

**B) `char` arrays (rows) cannot be assigned directly with `=`; their contents must be copied element-by-element via `strcpy`**

Just like an ordinary array variable, `arr[j]` (a row) decays to a non-assignable pointer-like expression; you cannot use `=` to copy an entire array's contents in one statement. `strcpy` performs the actual character-by-character copy needed to move one string's content into another's storage.
</details>

---

### Q5. Which storage approach for a list of strings is generally preferred when the strings must be modified (e.g., converted to uppercase) after being read in?

A) An array of pointers initialized from string literals
B) A 2-D `char` array, where each row is a genuine, writable array
C) Neither approach supports modification
D) A single `char *` pointing to all strings concatenated together

<details>
<summary><b>Answer</b></summary>

**B) A 2-D `char` array, where each row is a genuine, writable array**

Each row of a 2-D `char` array is an independent, writable block of memory (not a read-only literal), making it safe to modify characters in place — unlike an array of pointers initialized directly from string literals, which point to typically read-only memory.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Compare the 2-D `char` array approach and the array-of-pointers approach for storing multiple strings, in terms of memory layout, efficiency, and mutability.

<details>
<summary><b>Model Answer</b></summary>

A **2-D `char` array** (`char names[N][WIDTH];`) allocates one single contiguous block of memory, with every row fixed at exactly `WIDTH` bytes, regardless of how long each actual string is. This is simple to declare and every row is a genuine, independently writable array — but it can **waste memory** when string lengths vary significantly (every row costs the same, even for very short strings).

An **array of pointers** (`char *names[N];`) instead stores just `N` addresses; each pointer can reference a separately, precisely-sized block of memory (often a compact string literal). This is more **memory-efficient** overall (no wasted padding), but if initialized directly from string literals, the pointed-to memory is often **read-only**, making the strings effectively immutable unless you separately allocate writable storage for each one.

**Summary:** 2-D arrays trade some memory efficiency for guaranteed mutability and simplicity; arrays of pointers trade mutability (when using literals) for memory efficiency.
</details>

---

### Q2. Why can't you sort an array of strings using ordinary relational operators like `<` directly on the array elements? What must be used instead?

<details>
<summary><b>Model Answer</b></summary>

When `arr` is a 2-D `char` array, `arr[i]` and `arr[j]` (individual rows) both decay to pointer-like values representing *addresses*, not the strings' actual character content. Comparing them with `<` or `>` would compare **memory addresses** (essentially arbitrary and meaningless for alphabetical ordering), not the lexicographic (dictionary) order of the text they contain.

Instead, string comparisons for sorting must use **`strcmp(arr[i], arr[j])`**, which walks through both strings character by character and returns a negative, zero, or positive result reflecting their actual lexicographic relationship — this is the only correct way to determine alphabetical ordering between two C strings.
</details>

---

### Q3. Explain, step-by-step, why attempting `names[0][0] = 'X';` can crash a program when `names` is declared as `char *names[3] = {"Alice", "Bob", "Carol"};`, but works fine when declared as `char names[3][20] = {"Alice", "Bob", "Carol"};`.

<details>
<summary><b>Model Answer</b></summary>

**With `char *names[3] = {"Alice", ...};`:** Each element of `names` is a pointer that has been initialized to point directly at a **string literal**. String literals are commonly placed by the compiler into a **read-only** section of memory (to allow sharing/optimization, and to catch accidental modification bugs). Attempting to write through such a pointer (`names[0][0] = 'X'`) tries to modify that read-only memory — this is undefined behaviour, and on many systems/compilers, it will cause a segmentation fault (crash) at runtime.

**With `char names[3][20] = {"Alice", ...};`:** Here, `names` is a genuine 2-D array — real, independently-allocated, **writable** memory (typically on the stack), simply *initialized* with the given literal values copied into it at declaration time. There is no read-only string-literal memory involved once the array itself is created; `names[0][0] = 'X'` simply modifies this ordinary array's first byte, which is entirely legal.
</details>

---

### Q4. Write and explain a function `int countLongerThan(char arr[][20], int n, int length)` that returns how many strings in the array have more than `length` characters, using `strlen`.

<details>
<summary><b>Model Answer</b></summary>

```c
#include <string.h>

int countLongerThan(char arr[][20], int n, int length)
{
    int i, count = 0;
    for (i = 0; i < n; i++)
    {
        if (strlen(arr[i]) > length)
            count++;
    }
    return count;
}
```

**Explanation:** The function takes a 2-D `char` array (with a fixed column width of 20, matching the caller's declared array), the number of strings `n`, and a threshold `length`. It loops through each row (`arr[i]`, a full string), computes its length with `strlen`, and increments `count` whenever that length exceeds the given threshold. Finally, it returns the total count — a straightforward application of iterating over multiple strings and applying a per-string check via a standard library function.
</details>

---

### Q5. Describe the linear-search algorithm for finding a specific name within an array of strings, explaining why `strcmp` (not `==`) must be used for the comparison inside the loop.

<details>
<summary><b>Model Answer</b></summary>

To search for a target string `key` within an array of strings `arr[0..n-1]`, the algorithm examines each string one at a time, from index `0` up to `n-1`, comparing each `arr[i]` against `key`. If a match is found, the search stops immediately and reports the matching index; if the loop completes without finding a match, the search reports "not found" (conventionally, `-1`).

```c
int findName(char arr[][30], int n, char key[])
{
    int i;
    for (i = 0; i < n; i++)
        if (strcmp(arr[i], key) == 0)   /* strcmp compares CONTENT, character by character */
            return i;
    return -1;
}
```

**Why `strcmp`, not `==`:** `arr[i]` and `key` both decay to addresses (pointers) when used in an expression. Using `==` would compare whether those two addresses are numerically identical — almost never true even when the underlying text is exactly the same, since they typically live in entirely different memory locations. `strcmp` instead walks through the actual characters of both strings and only returns `0` when every corresponding character matches (and both strings end at the same length), which is the correct definition of "the same string content."
</details>
