# Supplementary Topic — Lecture: Dynamic Memory Allocation

## 📖 References

- *Let Us C*, 19th Edition — Chapter 13 ("Flexible Arrays", using `malloc()`), Chapter 15 (allocating space for a string), Chapter 23 (FAQs on `calloc()`, `realloc()` and `free()`)
- *Programming with C* (Byron Gottfried) — Section 10.5: "Dynamic Memory Allocation"

> **Prerequisites:** Pointers (Chapter 9), Arrays (Chapter 13), Strings (Chapters 15–16), Structures (Chapter 17).

---

## D.1 Why Do We Need Dynamic Memory?

Every array we have written so far has had its size fixed **when the program was written**:

```c
int marks[100];    /* always 100 ints, whether the class has 12 students or 400 */
```

This creates two problems:

1. **Waste:** if only 12 students are entered, the space for 88 integers is never used.
2. **Overflow:** if 400 students turn up, the array is simply too small, and writing past `marks[99]` is undefined behaviour (Chapter 13).

C99's variable-length arrays (`int arr[n];`, Chapter 13) help a little, but they live on the **stack**, which is small (often a few MB), and they disappear as soon as the function that created them returns.

**Dynamic memory allocation (DMA)** solves both problems: the program asks for exactly as much memory as it needs **while it is running**, and it can keep, grow, shrink or release that memory whenever it likes.

### D.1.1 Where Memory Comes From: Stack vs Heap

| | Stack | Heap |
|---|---|---|
| Used for | Local variables, function parameters, VLAs | Memory requested with `malloc()`, `calloc()`, `realloc()` |
| Size decided | At compile time (or at block entry for VLAs) | At run time, by the program |
| Typical capacity | Small (a few MB) | Large (limited by available RAM) |
| Lifetime | Ends automatically when the function returns | Lasts until the program calls `free()` (or exits) |
| Who releases it | The compiler, automatically | **The programmer**, explicitly |

> **Static vs dynamic allocation (exam definition):** In *static* allocation, the amount of memory is fixed at compile time. In *dynamic* allocation, memory is requested and released during execution using library functions such as `malloc()`, `calloc()`, `realloc()` and `free()`.

All four functions are declared in **`<stdlib.h>`**, so every program in this lecture includes it.

## D.2 `malloc()` — Allocating a Block

```c
void *malloc(size_t size);
```

- Takes the **number of bytes** to allocate.
- Returns the **base address** of the allocated block as a `void *` (a "generic" pointer that can be assigned to any pointer type).
- Returns **`NULL`** if the memory cannot be allocated.
- The contents of the block are **not initialized**: they hold garbage values.

```c
int *p;
p = (int *) malloc(5 * sizeof(int));   /* room for exactly 5 ints */
```

### D.2.1 Always Use `sizeof`

Never hard-code byte counts like `malloc(20)` for 5 integers. The size of an `int` is not guaranteed to be 4 on every machine. `n * sizeof(int)` is always correct.

### D.2.2 To Cast or Not to Cast?

*Let Us C* writes `(int *) malloc(...)`. In standard C, a `void *` is converted to any other object pointer type automatically, so

```c
int *p = malloc(5 * sizeof(int));
```

is equally correct. Both forms are accepted in this course. (The cast is **required** in C++, which is why many books include it.)

### D.2.3 Always Check for `NULL`

If the system cannot find enough memory, `malloc()` returns `NULL`. Using a `NULL` pointer as if it were an array crashes the program, so check every allocation:

```c
int *p = (int *) malloc(n * sizeof(int));
if (p == NULL)
{
    printf("Memory allocation failed\n");
    return 1;
}
```

### D.2.4 Using the Block as an Array

Once `p` holds the base address, the block is used **exactly like an array**. Both notations from Chapter 13 work:

```c
p[0] = 10;          /* array notation   */
*(p + 1) = 20;      /* pointer notation */
```

### D.2.5 A Complete Example

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int n, i, *p;

    printf("How many numbers? ");
    scanf("%d", &n);

    p = (int *) malloc(n * sizeof(int));
    if (p == NULL)
    {
        printf("Memory allocation failed\n");
        return 1;
    }

    for (i = 0; i < n; i++)
        p[i] = i * i;

    for (i = 0; i < n; i++)
        printf("%d ", p[i]);
    printf("\n");

    free(p);    /* give the memory back (see D.3) */
    return 0;
}
```

**Sample run:**
```
How many numbers? 6
0 1 4 9 16 25
```

## D.3 `free()` — Releasing a Block

```c
void free(void *ptr);
```

Heap memory is **not** released automatically when a function returns. Each block obtained from `malloc()`, `calloc()` or `realloc()` must be handed back with **exactly one** call to `free()`, passing the address that the allocation function returned.

```c
free(p);
p = NULL;   /* good habit: p no longer points to the freed block */
```

### D.3.1 Memory Leaks

A **memory leak** happens when the program loses the only pointer to a block that has not been freed. That memory cannot be used or released again until the program ends.

```c
int *p = malloc(100 * sizeof(int));
p = malloc(200 * sizeof(int));   /* LEAK: the first block of 100 ints is now unreachable */
```

A small leak in a short program does little harm, because the operating system reclaims everything when the program exits. In a long-running program (a server, an operating system, a game loop) leaks keep piling up until memory runs out.

### D.3.2 Dangling Pointers

After `free(p)`, the pointer `p` still holds the old address, but that memory no longer belongs to the program. `p` is now a **dangling pointer**.

```c
free(p);
printf("%d\n", p[0]);   /* UNDEFINED BEHAVIOUR: using memory after it was freed */
free(p);                /* UNDEFINED BEHAVIOUR: freeing the same block twice    */
```

Setting `p = NULL;` immediately after `free(p);` protects against both mistakes: `free(NULL)` is defined to do nothing, and an accidental `p[0]` on a `NULL` pointer fails immediately instead of silently corrupting data.

### D.3.3 Only Free What You Allocated

```c
int arr[10];
free(arr);    /* WRONG: arr is on the stack, not from malloc() */
```

## D.4 `calloc()` — Allocating a Zero-Filled Block

```c
void *calloc(size_t count, size_t size);
```

`calloc()` allocates space for an array of `count` elements, each `size` bytes, and **sets every byte to zero**.

```c
int *p = (int *) calloc(10, sizeof(int));   /* 10 ints, all equal to 0 */
```

| | `malloc()` | `calloc()` |
|---|---|---|
| Arguments | 1: total bytes | 2: number of elements, size of each |
| Initial contents | Garbage (uninitialized) | All zero |
| Typical call | `malloc(n * sizeof(int))` | `calloc(n, sizeof(int))` |
| Freed with | `free()` | `free()` |

Use `calloc()` when you want counters, totals or flags that must start at zero, for example a frequency table.

## D.5 `realloc()` — Resizing a Block

```c
void *realloc(void *ptr, size_t new_size);
```

`realloc()` changes the size of a block that was previously allocated:

- The old contents are **preserved** (up to the smaller of the old and new sizes).
- If the block can grow in place, the same address is returned. Otherwise a new block is allocated, the data is copied across, the old block is freed, and the **new address** is returned.
- On failure it returns `NULL` and the **original block is left untouched**.

### D.5.1 The Safe Pattern

```c
int *temp = (int *) realloc(p, new_n * sizeof(int));
if (temp == NULL)
{
    printf("Could not grow the array\n");
    /* p is still valid here; keep using it or free it */
}
else
{
    p = temp;    /* only overwrite p once we know realloc succeeded */
}
```

> **Why not write `p = realloc(p, ...)` directly?** If `realloc()` fails, it returns `NULL`, `p` gets overwritten with `NULL`, and the original block (still allocated) is lost forever: a memory leak.

Two special cases worth knowing: `realloc(NULL, size)` behaves like `malloc(size)`, and after a successful `realloc()` **any other pointer** into the old block must be treated as dangling, since the block may have moved.

## D.6 Returning Memory from a Function

A function must **never** return the address of its own local array, because that array is destroyed when the function returns:

```c
int *makeArray(int n)       /* WRONG */
{
    int arr[100];
    /* ... */
    return arr;             /* dangling: arr no longer exists after the return */
}
```

Heap memory survives the return, so this version is correct:

```c
int *makeArray(int n)       /* CORRECT */
{
    int *arr = (int *) malloc(n * sizeof(int));
    int i;
    if (arr == NULL)
        return NULL;
    for (i = 0; i < n; i++)
        arr[i] = i + 1;
    return arr;             /* the caller is now responsible for calling free() */
}
```

> **Ownership rule:** whoever receives a pointer to heap memory is responsible for freeing it. Write this down in a comment above any function that returns allocated memory.

## D.7 Dynamic Strings

Chapter 16 showed that an array of pointers to strings cannot safely receive input with `scanf()` unless each pointer first points at real memory. `malloc()` lets us allocate exactly the right amount for each string: **`strlen + 1` bytes**, the extra byte holding the terminating `'\0'`.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main()
{
    char buffer[100];
    char *name;

    printf("Enter your name: ");
    scanf("%99s", buffer);

    name = (char *) malloc(strlen(buffer) + 1);   /* +1 for '\0' */
    if (name == NULL)
        return 1;

    strcpy(name, buffer);
    printf("Stored \"%s\" using %d bytes\n", name, (int)(strlen(name) + 1));

    free(name);
    return 0;
}
```

**Sample run:**
```
Enter your name: Ananya
Stored "Ananya" using 7 bytes
```

Forgetting the `+ 1` is one of the most common DMA bugs: `strcpy()` writes the `'\0'` one byte past the end of the block.

## D.8 Dynamic Structures

`sizeof` works on structure types too (Chapter 17), so a single record or a whole array of records can be allocated at run time. Members are reached with the arrow operator `->`, or with `[i].` for an array.

```c
struct Student
{
    char name[30];
    int roll;
    float marks;
};

struct Student *s = (struct Student *) malloc(sizeof(struct Student));
s->roll = 101;                 /* same as (*s).roll = 101; */

struct Student *list = (struct Student *) malloc(n * sizeof(struct Student));
list[2].marks = 88.5;          /* list[2] is a struct, so use '.' */
```

This is the basis of linked lists, trees and every other dynamic data structure you will meet in a Data Structures course: each node is a structure allocated with `malloc()` that holds a pointer to the next node.

## D.9 Dynamic 2-D Arrays (Array of Row Pointers)

To create an `r × c` matrix whose size is known only at run time, allocate an array of `r` row pointers, then allocate each row separately:

```c
int **m = (int **) malloc(r * sizeof(int *));
for (i = 0; i < r; i++)
    m[i] = (int *) malloc(c * sizeof(int));

m[1][2] = 7;    /* used exactly like a normal 2-D array */

/* free in reverse order: rows first, then the array of row pointers */
for (i = 0; i < r; i++)
    free(m[i]);
free(m);
```

Freeing `m` first would lose the addresses of the rows, leaking all of them.

## D.10 Worked Programs

### Program 1: Average of N Numbers (Size Chosen at Run Time)

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int n, i;
    float *arr, sum = 0;

    printf("Enter the number of values: ");
    scanf("%d", &n);

    arr = (float *) malloc(n * sizeof(float));
    if (arr == NULL)
    {
        printf("Memory allocation failed\n");
        return 1;
    }

    printf("Enter %d values: ", n);
    for (i = 0; i < n; i++)
    {
        scanf("%f", &arr[i]);
        sum += arr[i];
    }

    printf("Average = %.2f\n", sum / n);

    free(arr);
    return 0;
}
```

**Sample run:**
```
Enter the number of values: 4
Enter 4 values: 10 20 30 45
Average = 26.25
```

### Program 2: A Growing Array with `realloc()`

The user enters positive numbers until `-1`; the program does not know in advance how many there will be. The array starts with room for 2 numbers and **doubles** its capacity whenever it fills up.

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int capacity = 2, count = 0, x, i;
    int *arr = (int *) malloc(capacity * sizeof(int));
    int *temp;

    if (arr == NULL)
        return 1;

    printf("Enter numbers (-1 to stop): ");
    while (scanf("%d", &x) == 1 && x != -1)
    {
        if (count == capacity)
        {
            capacity = capacity * 2;
            temp = (int *) realloc(arr, capacity * sizeof(int));
            if (temp == NULL)
            {
                free(arr);
                return 1;
            }
            arr = temp;
            printf("[capacity grown to %d]\n", capacity);
        }
        arr[count] = x;
        count++;
    }

    printf("You entered %d numbers: ", count);
    for (i = 0; i < count; i++)
        printf("%d ", arr[i]);
    printf("\n");

    free(arr);
    return 0;
}
```

**Sample run:**
```
Enter numbers (-1 to stop): 5 10 15 20 25 -1
[capacity grown to 4]
[capacity grown to 8]
You entered 5 numbers: 5 10 15 20 25
```

Doubling (rather than growing by 1 each time) keeps the number of `realloc()` calls small, since each call may need to copy the whole array.

### Program 3: Frequency Count Using `calloc()`

Counts how many times each digit 0–9 appears in a number. `calloc()` guarantees every counter starts at 0.

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    long num;
    int i;
    int *count = (int *) calloc(10, sizeof(int));

    if (count == NULL)
        return 1;

    printf("Enter a number: ");
    scanf("%ld", &num);

    if (num == 0)
        count[0] = 1;
    while (num > 0)
    {
        count[num % 10]++;
        num = num / 10;
    }

    for (i = 0; i < 10; i++)
        if (count[i] > 0)
            printf("Digit %d appears %d time(s)\n", i, count[i]);

    free(count);
    return 0;
}
```

**Sample run:**
```
Enter a number: 1223334
Digit 1 appears 1 time(s)
Digit 2 appears 2 time(s)
Digit 3 appears 3 time(s)
Digit 4 appears 1 time(s)
```

### Program 4: Dynamic Array of Structures

```c
#include <stdio.h>
#include <stdlib.h>

struct Student
{
    char name[30];
    float marks;
};

int main()
{
    int n, i, top = 0;
    struct Student *s;

    printf("Number of students: ");
    scanf("%d", &n);

    s = (struct Student *) malloc(n * sizeof(struct Student));
    if (s == NULL)
        return 1;

    for (i = 0; i < n; i++)
    {
        printf("Name and marks of student %d: ", i + 1);
        scanf("%29s %f", s[i].name, &s[i].marks);
        if (s[i].marks > s[top].marks)
            top = i;
    }

    printf("Topper: %s with %.1f marks\n", s[top].name, s[top].marks);

    free(s);
    return 0;
}
```

**Sample run:**
```
Number of students: 3
Name and marks of student 1: Arjun 71
Name and marks of student 2: Diya 89.5
Name and marks of student 3: Kabir 84
Topper: Diya with 89.5 marks
```

## D.11 Common Pitfalls

| Pitfall | Example | Fix |
|---|---|---|
| Forgetting `<stdlib.h>` | Calling `malloc()` without the header | Always `#include <stdlib.h>` |
| Allocating elements instead of bytes | `malloc(n)` for `n` ints | `malloc(n * sizeof(int))` |
| Not checking for `NULL` | Using `p[0]` straight after `malloc()` | `if (p == NULL) { ... }` |
| Assuming `malloc()` zero-fills | Using a `malloc()`'d array as counters | Initialize it yourself, or use `calloc()` |
| Memory leak | Reassigning the only pointer to a block | `free()` the old block first |
| Use after free | Reading `p[0]` after `free(p)` | Set `p = NULL` after freeing |
| Double free | `free(p); free(p);` | Set `p = NULL` after freeing |
| `p = realloc(p, ...)` | Original block lost if `realloc()` fails | Assign to a temporary pointer first |
| Missing `+ 1` for strings | `malloc(strlen(s))` | `malloc(strlen(s) + 1)` |
| Freeing stack memory | `int a[5]; free(a);` | Only free what `malloc`/`calloc`/`realloc` returned |
| Returning a local array | `int a[10]; ... return a;` | Return heap memory and document who frees it |

## D.12 Key Takeaways

1. Dynamic memory is requested from the **heap** at run time; its size can depend on user input and it lives until `free()` is called.
2. `malloc(bytes)` returns uninitialized memory; `calloc(count, size)` returns zero-filled memory; both return `NULL` on failure.
3. `realloc(p, new_size)` resizes a block, keeping its contents, and may move it to a new address; always assign its result to a temporary pointer first.
4. Every successful allocation needs exactly one matching `free()`. Forgetting it causes a **memory leak**; using the pointer afterwards is a **dangling pointer** bug.
5. Always size allocations with `sizeof`, always check for `NULL`, and allocate `strlen + 1` bytes for strings.
6. A dynamically allocated block is used exactly like an array: `p[i]` and `*(p + i)` both work.
