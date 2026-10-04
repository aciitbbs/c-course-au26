# Supplementary Topic — Quiz: Dynamic Memory Allocation

## 📖 Topics Covered: Stack vs heap, `malloc()`, `calloc()`, `realloc()`, `free()`, memory leaks, dangling pointers, dynamic strings and structures

---

## Part A: Multiple Choice Questions (5)

### Q1. Which header file must be included to use `malloc()`, `calloc()`, `realloc()` and `free()`?

A) `<stdio.h>`
B) `<string.h>`
C) `<stdlib.h>`
D) `<memory.h>`

<details>
<summary><b>Answer</b></summary>

**C) `<stdlib.h>`**

All four dynamic memory functions are declared in the standard library header `<stdlib.h>`. Without it, the compiler does not know their prototypes and will warn about (or reject) the calls.
</details>

---

### Q2. What does `malloc()` return if it is unable to allocate the requested memory?

A) `0` bytes of memory at a valid address
B) `NULL`
C) `-1`
D) The program terminates automatically

<details>
<summary><b>Answer</b></summary>

**B) `NULL`**

On failure, `malloc()` (and likewise `calloc()` and `realloc()`) returns a null pointer. This is why every allocation should be followed by a check such as `if (p == NULL)` before the memory is used.
</details>

---

### Q3. Which statement correctly allocates space for an array of `n` integers, with every element initialized to `0`?

A) `int *p = malloc(n);`
B) `int *p = malloc(n * sizeof(int));`
C) `int *p = calloc(n, sizeof(int));`
D) `int *p = calloc(n * sizeof(int));`

<details>
<summary><b>Answer</b></summary>

**C) `int *p = calloc(n, sizeof(int));`**

`calloc()` takes two arguments (number of elements, size of each) and zero-fills the block. Option B allocates the right amount but leaves the contents uninitialized (garbage). Option A allocates only `n` bytes, not `n` integers. Option D passes only one argument to `calloc()`, which is a compile error.
</details>

---

### Q4. Consider the code below. What is the problem?

```c
int *p = malloc(10 * sizeof(int));
p = malloc(20 * sizeof(int));
```

A) Dangling pointer
B) Memory leak
C) Double free
D) Nothing is wrong

<details>
<summary><b>Answer</b></summary>

**B) Memory leak**

The address of the first block (10 integers) was stored only in `p`. Overwriting `p` with the address of the second block means the first block can never be accessed or freed again. The fix is to call `free(p);` before the second `malloc()`, or to use `realloc()` if the goal is to resize.
</details>

---

### Q5. After the statement `free(p);` executes, which of the following is true?

A) `p` is automatically set to `NULL`
B) `p` still holds the old address, but accessing that memory is undefined behaviour
C) The memory is still safely usable until the function returns
D) `p` now points to a new block of the same size

<details>
<summary><b>Answer</b></summary>

**B) `p` still holds the old address, but accessing that memory is undefined behaviour**

`free()` cannot change the caller's pointer variable (it receives a copy of the address). `p` becomes a **dangling pointer**. Writing `p = NULL;` immediately afterwards is a good habit that prevents accidental use-after-free and double-free bugs.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Distinguish between static and dynamic memory allocation. Give one advantage of each.

<details>
<summary><b>Model Answer</b></summary>

In **static memory allocation**, the amount of memory a variable or array needs is fixed when the program is compiled (e.g., `int marks[100];`). It is reserved automatically (on the stack for local variables) and released automatically when the variable goes out of scope.

In **dynamic memory allocation**, memory is requested **at run time** from the heap using `malloc()`, `calloc()` or `realloc()`, and must be released explicitly with `free()`.

- **Advantage of static allocation:** simple and safe: no need to check for allocation failure or remember to free anything, and access is fast.
- **Advantage of dynamic allocation:** the size can depend on data known only at run time (such as user input), so memory is neither wasted nor too small, the block can be resized with `realloc()`, and it can outlive the function that created it.
</details>

---

### Q2. Compare `malloc()` and `calloc()` with respect to their arguments, the initial contents of the allocated memory, and how the memory is released.

<details>
<summary><b>Model Answer</b></summary>

| | `malloc()` | `calloc()` |
|---|---|---|
| Arguments | One: the total number of bytes, e.g. `malloc(n * sizeof(int))` | Two: the number of elements and the size of each, e.g. `calloc(n, sizeof(int))` |
| Initial contents | Uninitialized (garbage values) | Every byte set to zero |
| Release | `free(p)` | `free(p)` (the same function) |

Both return a `void *` to the start of the block, or `NULL` on failure. `calloc()` is convenient when the elements must start at zero (counters, sums, flags); `malloc()` avoids the cost of zeroing when the program will overwrite every element anyway.
</details>

---

### Q3. Why is the statement `p = realloc(p, 2 * n * sizeof(int));` considered unsafe? Rewrite it safely.

<details>
<summary><b>Model Answer</b></summary>

If `realloc()` cannot find enough memory, it returns `NULL` but leaves the original block allocated and unchanged. Assigning the result straight to `p` overwrites the only pointer to that original block with `NULL`. The original data can no longer be accessed or freed, which is a memory leak (and the program has also lost its data).

**Safe version:**
```c
int *temp = realloc(p, 2 * n * sizeof(int));
if (temp == NULL)
{
    printf("Reallocation failed\n");
    /* p is still valid: continue with the old size, or free(p) */
}
else
{
    p = temp;
    n = 2 * n;
}
```
</details>

---

### Q4. Identify all the errors in the following function and rewrite it correctly.

```c
char *copyString(char *src)
{
    char *dest = malloc(strlen(src));
    strcpy(dest, src);
    return dest;
}
```

<details>
<summary><b>Model Answer</b></summary>

1. **Missing space for `'\0'`:** `strlen(src)` does not count the terminating null character, so `strcpy()` writes one byte past the end of the block (undefined behaviour). It should be `malloc(strlen(src) + 1)`.
2. **No `NULL` check:** if `malloc()` fails, `strcpy()` writes to a null pointer and the program crashes.

(Also make sure `<stdlib.h>` and `<string.h>` are included.)

**Corrected version:**
```c
char *copyString(char *src)
{
    char *dest = malloc(strlen(src) + 1);   /* +1 for '\0' */
    if (dest == NULL)
        return NULL;
    strcpy(dest, src);
    return dest;    /* the caller must free() this */
}
```

Returning `dest` is correct here: heap memory, unlike a local array, survives after the function returns. The caller becomes responsible for freeing it.
</details>

---

### Q5. Write a program that reads an integer `n`, dynamically allocates an array of `n` integers, reads `n` values into it, prints them in reverse order, and releases the memory.

<details>
<summary><b>Model Answer</b></summary>

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int n, i;
    int *arr;

    printf("Enter n: ");
    scanf("%d", &n);

    arr = (int *) malloc(n * sizeof(int));
    if (arr == NULL)
    {
        printf("Memory allocation failed\n");
        return 1;
    }

    printf("Enter %d integers: ", n);
    for (i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    printf("Reversed: ");
    for (i = n - 1; i >= 0; i--)
        printf("%d ", arr[i]);
    printf("\n");

    free(arr);
    arr = NULL;
    return 0;
}
```

**Sample run:**
```
Enter n: 5
Enter 5 integers: 3 8 1 9 4
Reversed: 4 9 1 8 3
```

Key points: `<stdlib.h>` is included, the size uses `sizeof(int)`, the result is checked for `NULL`, the block is used with ordinary array notation, and it is freed exactly once.
</details>
