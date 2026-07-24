# Chapter 13 — Quiz: Arrays

## 📖 Topics Covered: Array declaration/initialization, bounds, array-pointer equivalence, passing arrays to functions

---

## Part A: Multiple Choice Questions (5)

### Q1. Given `int arr[5];`, what are the valid indices for accessing its elements?

A) `1` to `5`
B) `0` to `4`
C) `0` to `5`
D) `-1` to `4`

<details>
<summary><b>Answer</b></summary>

**B) `0` to `4`**

C arrays are zero-indexed; a 5-element array has valid indices `0, 1, 2, 3, 4`. Accessing `arr[5]` is out of bounds and undefined behaviour.
</details>

---

### Q2. What happens when a program accesses `arr[10]` on an array declared as `int arr[5];`?

A) The compiler refuses to compile
B) A runtime error/exception is raised automatically
C) It is undefined behaviour — C performs no automatic bounds checking
D) `arr[10]` is automatically treated as `arr[0]`

<details>
<summary><b>Answer</b></summary>

**C) It is undefined behaviour — C performs no automatic bounds checking**

Unlike some higher-level languages, C never checks array bounds at compile time or runtime. Accessing an out-of-bounds index silently reads/writes whatever memory happens to be there, which can corrupt data or crash the program, without any built-in warning.
</details>

---

### Q3. Given `int arr[4] = {10, 20, 30, 40}; int *p = arr;`, what does `*(p + 2)` evaluate to?

A) `10`
B) `20`
C) `30`
D) The address of `arr[2]`

<details>
<summary><b>Answer</b></summary>

**C) `30`**

`p` points to `arr[0]`. `p + 2` advances the pointer by 2 elements (automatically scaled by `sizeof(int)`), pointing to `arr[2]`. Dereferencing with `*` gives that element's value, `30`.
</details>

---

### Q4. Why must the size of an array typically be passed as a separate parameter to a function that receives the array?

A) C requires all functions to have exactly two parameters
B) Because inside the function, the array parameter is really just a pointer, and `sizeof` on it gives the pointer's size, not the array's element count
C) Arrays cannot be passed to functions at all without a size parameter — it's a syntax requirement
D) It is not actually necessary; the function can always determine the array's size automatically

<details>
<summary><b>Answer</b></summary>

**B) Because inside the function, the array parameter is really just a pointer, and `sizeof` on it gives the pointer's size, not the array's element count**

When an array is passed to a function, it decays into a pointer to its first element. Inside the function, there is no built-in way to recover the original array's length from that pointer alone — the caller must explicitly pass the count as an additional parameter.
</details>

---

### Q5. What is the result of `int arr[3] = {5, 10, 15}; arr = arr + 1;`?

A) `arr` now points to `{10, 15}` and works fine
B) Compilation error — array names cannot be reassigned like ordinary pointer variables
C) `arr[0]` becomes `10`
D) The program crashes at runtime

<details>
<summary><b>Answer</b></summary>

**B) Compilation error — array names cannot be reassigned like ordinary pointer variables**

While an array's name decays to a pointer value when *used* in an expression, the array name itself is not an assignable (modifiable lvalue) pointer variable — you cannot make it point elsewhere. A genuine pointer variable (e.g., `int *p = arr; p = p + 1;`) can be reassigned freely, but the array identifier itself cannot.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain the relationship between array notation (`arr[i]`) and pointer notation (`*(arr + i)`). Are they always interchangeable? Explain any subtlety.

<details>
<summary><b>Model Answer</b></summary>

For any array `arr` and valid index `i`, `arr[i]` and `*(arr + i)` are **guaranteed to produce the same value** — this is because C defines `arr[i]` as syntactic sugar for exactly `*(arr + i)`: the array name decays to a pointer to its first element, and adding `i` (automatically scaled by the element's `sizeof`) advances to the `i`-th element, which is then dereferenced.

**Subtlety:** while they are interchangeable for *reading and writing individual elements*, the array name `arr` itself is **not** a modifiable pointer variable — expressions like `arr = arr + 1;` are illegal, whereas an ordinary pointer variable initialized to point at the array (`int *p = arr;`) **can** be reassigned (`p = p + 1;`). So the equivalence applies to *element access*, not to the identifier's general behaviour as a variable.
</details>

---

### Q2. Why does C provide no automatic array bounds checking, and what practical discipline must a C programmer follow as a result?

<details>
<summary><b>Model Answer</b></summary>

C was designed for maximum performance and minimal runtime overhead, closely mirroring how the underlying hardware works — adding automatic bounds checking to every array access would add a runtime cost (an extra comparison on every access) that the language's designers chose not to impose by default, leaving performance-critical code free of that overhead.

As a direct consequence, the **programmer bears full responsibility** for ensuring every array access stays within `[0, size-1]`. This means always double-checking loop conditions (`i < size`, not `i <= size`), being especially careful with off-by-one boundaries, and validating any user-supplied index before using it to access an array — since C will not catch these mistakes automatically, and the resulting bugs (memory corruption, crashes, or worse — seemingly correct output that hides a real problem) can be extremely difficult to trace later.
</details>

---

### Q3. Describe what happens, step by step, when the array `int nums[5] = {1,2,3,4,5};` is passed to a function `void process(int nums[], int size)`. What is actually copied, and what is shared?

<details>
<summary><b>Model Answer</b></summary>

When `process(nums, 5)` is called, the array name `nums` decays into a pointer holding the **address of `nums[0]`**. It is this **address (pointer value)** that is actually copied and passed to the function's `nums` parameter — not the array's contents.

As a result:
- The **pointer itself** (the parameter `nums` inside `process`) is a local copy — reassigning it inside the function (e.g., `nums = someOtherArray;`) would not affect the caller's variable.
- However, since both the caller's array and the function's parameter *point to the exact same underlying memory* (the original 5 integers), any modification made through `nums[i] = ...;` **inside** the function directly changes the caller's original array — the actual element data is shared, not copied.

This is exactly why array modifications inside a function are visible back in the caller, even though C is normally "call by value" — arrays achieve an effective "call by reference" through this pointer-decay mechanism.
</details>

---

### Q4. Trace the following program and give its complete output.

```c
#include <stdio.h>
int main()
{
    int arr[5] = {2, 4, 6, 8, 10};
    int *p = arr;
    int i;

    for (i = 0; i < 5; i++)
        printf("%d ", *(p + i));
    printf("\n");

    for (i = 0; i < 5; i++)
        *(p + i) = *(p + i) + 1;

    for (i = 0; i < 5; i++)
        printf("%d ", arr[i]);
    printf("\n");

    return 0;
}
```

<details>
<summary><b>Model Answer</b></summary>

**Output:**
```
2 4 6 8 10 
3 5 7 9 11 
```

Trace: The first loop prints each element via pointer dereferencing (`*(p+i)`), giving the original values. The second loop increments each element **through the pointer**, which directly modifies the underlying `arr` array (since `p` points to the same memory as `arr`). The third loop then prints `arr[i]` directly, confirming that each element has indeed increased by 1.
</details>

---

### Q5. What is a variable-length array (VLA), introduced in C99? Give an example, and explain one practical risk of using very large VLAs.

<details>
<summary><b>Model Answer</b></summary>

A **variable-length array (VLA)** is an array whose size is determined by a value computed **at runtime**, rather than being a fixed compile-time constant — a feature introduced in the C99 standard.

```c
int n;
scanf("%d", &n);
int arr[n];    /* size 'n' is only known once the program actually runs */
```

**Practical risk:** VLAs are typically allocated on the **stack**, which has a comparatively small, fixed total size (often just a few megabytes). If the runtime-determined size `n` turns out to be very large (e.g., due to unchecked or malicious user input), the VLA can exceed the available stack space, causing a **stack overflow** and crashing the program. For genuinely large or dynamically-sized data, dynamic memory allocation on the heap (via `malloc`, a topic introduced later) is generally the safer, more scalable choice.
</details>
