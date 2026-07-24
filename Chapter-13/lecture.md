# Chapter 13 — Lecture: Arrays

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 13: "Arrays"

---

## 13.1 What are Arrays?

An **array** is a collection of a **fixed number of elements**, all of the **same data type**, stored in **contiguous memory locations**, and accessed using a common name and an **index (subscript)**.

```c
int marks[5];    /* an array of 5 integers, named 'marks' */
```

- Indexing in C is **zero-based**: valid indices for `marks[5]` are `marks[0]` through `marks[4]` — **not** `marks[5]`.
- All elements occupy one contiguous block of memory, each `sizeof(type)` bytes wide, laid out sequentially.

### 13.1.1 A Simple Program Using an Array

```c
#include <stdio.h>

int main()
{
    int marks[5];
    int i;

    for (i = 0; i < 5; i++)
    {
        printf("Enter marks for subject %d: ", i + 1);
        scanf("%d", &marks[i]);
    }

    printf("\nMarks entered:\n");
    for (i = 0; i < 5; i++)
        printf("Subject %d: %d\n", i + 1, marks[i]);

    return 0;
}
```

## 13.2 More on Arrays

### 13.2.1 Array Initialization

```c
int marks[5] = {90, 85, 78, 92, 88};      /* fully initialized */
int scores[5] = {90, 85};                  /* partial init: remaining elements are 0 */
int zeros[5] = {0};                        /* all elements set to 0 */
int auto_size[] = {1, 2, 3, 4};            /* size (4) is inferred automatically from the initializer list */
```

### 13.2.2 Array Elements in Memory

An array's elements occupy **consecutive** memory addresses. If `marks[0]` is at address `1000` and each `int` is 4 bytes, `marks[1]` is at `1004`, `marks[2]` at `1008`, and so on — this predictable layout is exactly what makes pointer arithmetic on arrays possible (see §13.3).

### 13.2.3 Bounds Checking

> **C performs NO automatic array bounds checking.** Accessing `marks[10]` on a 5-element array does **not** raise an error — it silently reads/writes whatever memory happens to lie past the array, causing **undefined behaviour** (corrupted data, crashes, or — worst of all — code that appears to "work" while being fundamentally broken).

```c
int arr[5] = {1, 2, 3, 4, 5};
arr[10] = 99;    /* UNDEFINED BEHAVIOUR: no error, no warning at runtime -- silently corrupts memory */
```

Always ensure loop bounds match the array's actual declared size exactly (`i < 5`, never `i <= 5`, for a 5-element array).

### 13.2.4 Passing Array Elements to a Function

Individual elements can be passed just like ordinary variables (by value):

```c
void printValue(int x)
{
    printf("%d\n", x);
}
/* call: printValue(marks[2]); */
```

## 13.3 Pointers and Arrays

This is one of the most important — and most tested — relationships in C: **the name of an array, used by itself, decays into a pointer to its first element.**

```c
int arr[5] = {10, 20, 30, 40, 50};
printf("%p\n", arr);        /* same address as... */
printf("%p\n", &arr[0]);    /* ...this -- arr is equivalent to &arr[0] */
```

### 13.3.1 Accessing Array Elements Using Pointers

```c
int arr[5] = {10, 20, 30, 40, 50};
int *p = arr;    /* p now points to arr[0]; no '&' needed since arr already decays to an address */

printf("%d\n", *p);        /* 10  (same as arr[0]) */
printf("%d\n", *(p + 1));  /* 20  (same as arr[1]) */
printf("%d\n", *(p + 2));  /* 30  (same as arr[2]) */
```

**Pointer arithmetic is automatically scaled** by the pointed-to type's size: `p + 1` does **not** mean "add 1 byte" — it means "advance by `sizeof(int)` bytes" (typically 4), landing exactly on the next `int` element. This is why array-pointer navigation works correctly regardless of the element type's size.

### The Equivalence Table

| Array notation | Pointer notation | Meaning |
|---|---|---|
| `arr[i]` | `*(arr + i)` | The value of the `i`-th element |
| `&arr[i]` | `arr + i` | The address of the `i`-th element |
| `arr` | `&arr[0]` | The address of the first element |

```c
int arr[5] = {10, 20, 30, 40, 50};
int i;
for (i = 0; i < 5; i++)
    printf("%d ", *(arr + i));   /* identical output to printf("%d ", arr[i]); */
```

> **Subtle but important distinction:** while `arr[i]` and `*(arr+i)` behave identically for reading/writing elements, `arr` itself is **not** a modifiable pointer variable — you cannot write `arr = arr + 1;` (array names are not assignable). A separate pointer variable (`int *p = arr;`) **can** be reassigned freely.

### 13.3.2 Passing an Array to a Function

Arrays are **always passed by reference** in C (technically: the array decays to a pointer to its first element, and that pointer is passed by value) — so a function receiving an array parameter can modify the caller's original array directly, unlike ordinary scalar variables.

```c
#include <stdio.h>

void doubleAll(int arr[], int size)   /* equivalently: void doubleAll(int *arr, int size) */
{
    int i;
    for (i = 0; i < size; i++)
        arr[i] = arr[i] * 2;
}

int main()
{
    int numbers[5] = {1, 2, 3, 4, 5};
    int i;

    doubleAll(numbers, 5);

    for (i = 0; i < 5; i++)
        printf("%d ", numbers[i]);   /* prints: 2 4 6 8 10 -- original array WAS modified */
    printf("\n");

    return 0;
}
```

> **Why must you always also pass the size?** Because inside the function, `arr` is just a pointer — `sizeof(arr)` there gives the size of a *pointer* (e.g., 8 bytes), **not** the size of the original array. The function has no way to know how many elements the array actually has unless told explicitly via a separate parameter.

## 13.4 Flexible Arrays

C99 introduced **variable-length arrays (VLAs)**, where an array's size can be determined by a runtime variable rather than a compile-time constant:

```c
#include <stdio.h>
int main()
{
    int n;
    printf("Enter size: ");
    scanf("%d", &n);

    int arr[n];    /* VLA: size determined at RUNTIME, valid from C99 onwards */

    int i;
    for (i = 0; i < n; i++)
        arr[i] = i * i;

    for (i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");

    return 0;
}
```

> VLAs are allocated on the **stack**, so very large `n` values can still cause a stack overflow — for genuinely large or dynamically-sized data, `malloc`-based dynamic memory allocation (a more advanced topic, briefly touched on in Chapter 24) is generally preferred.

## 13.5 Worked Programs

### Program 1: Linear Search

```c
#include <stdio.h>

int linearSearch(int arr[], int size, int key)
{
    int i;
    for (i = 0; i < size; i++)
    {
        if (arr[i] == key)
            return i;      /* found -- return the index */
    }
    return -1;              /* not found */
}

int main()
{
    int arr[6] = {4, 8, 15, 16, 23, 42};
    int key, index;

    printf("Enter a number to search: ");
    scanf("%d", &key);

    index = linearSearch(arr, 6, key);

    if (index != -1)
        printf("%d found at index %d.\n", key, index);
    else
        printf("%d not found.\n", key);

    return 0;
}
```

### Program 2: Finding Maximum and Minimum in an Array

```c
#include <stdio.h>

void findMinMax(int arr[], int size, int *min, int *max)
{
    int i;
    *min = arr[0];
    *max = arr[0];

    for (i = 1; i < size; i++)
    {
        if (arr[i] < *min) *min = arr[i];
        if (arr[i] > *max) *max = arr[i];
    }
}

int main()
{
    int arr[7] = {45, 12, 78, 3, 99, 27, 56};
    int min, max;

    findMinMax(arr, 7, &min, &max);

    printf("Minimum = %d\n", min);
    printf("Maximum = %d\n", max);

    return 0;
}
```

### Program 3: Bubble Sort (Ascending Order)

```c
#include <stdio.h>

void bubbleSort(int arr[], int size)
{
    int i, j, temp;
    for (i = 0; i < size - 1; i++)
    {
        for (j = 0; j < size - 1 - i; j++)
        {
            if (arr[j] > arr[j + 1])
            {
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

int main()
{
    int arr[6] = {64, 25, 12, 22, 11, 90};
    int i;

    bubbleSort(arr, 6);

    printf("Sorted array: ");
    for (i = 0; i < 6; i++)
        printf("%d ", arr[i]);
    printf("\n");

    return 0;
}
```

**Output:** `Sorted array: 11 12 22 25 64 90`

## 13.6 Common Pitfalls

| Pitfall | Example | Fix |
|---|---|---|
| Off-by-one bounds error | `for (i = 0; i <= size; i++)` on a `size`-element array | Use `i < size`, never `<=` |
| Forgetting to pass array size to a function | `void process(int arr[]) { ... uses sizeof(arr) ... }` | Always pass size as a separate parameter |
| Treating `arr` as a reassignable pointer | `arr = arr + 1;` | Array names cannot be reassigned; use a separate pointer variable instead |
| Assuming C checks array bounds | Accessing `arr[size]` expecting a runtime error | C never checks bounds — the programmer alone is responsible |

## 13.7 Key Takeaways

1. Arrays store a fixed number of same-typed elements contiguously in memory, indexed from `0` to `size-1`.
2. C performs **no bounds checking** — out-of-range access is undefined behaviour, not a caught error.
3. An array name decays into a pointer to its first element; `arr[i]` and `*(arr+i)` are equivalent.
4. Arrays are effectively passed by reference to functions (via pointer decay) — always pass the size separately, since the function cannot deduce it from the pointer alone.
5. Pointer arithmetic on arrays is automatically scaled by the element type's size, making `p+1` correctly advance to the *next* element, not the next byte.
