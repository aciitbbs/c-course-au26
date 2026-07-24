# Chapter 9 — Lecture: Pointers

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 9: "Pointers"

> This chapter is one of the **most important in the entire course**. A firm grip on pointers is essential for arrays, strings, dynamic memory, and virtually every advanced C topic that follows.

---

## 9.1 Call by Value and Call by Reference

Recall from Chapter 8 that function arguments are normally passed **by value** — the function receives a **copy** of the argument, so changes made inside the function do **not** affect the caller's original variable.

```c
#include <stdio.h>

void tryToDouble(int n)
{
    n = n * 2;              /* modifies only the LOCAL copy 'n' */
}

int main()
{
    int x = 5;
    tryToDouble(x);
    printf("x = %d\n", x);  /* still prints 5 -- x in main() is untouched */
    return 0;
}
```

To let a function modify the caller's actual variable, C uses **call by reference**, implemented via **pointers** — passing the *address* of the variable instead of its value.

## 9.2 An Introduction to Pointers

A **pointer** is a variable that stores the **memory address** of another variable, rather than an ordinary data value.

### The Two Key Operators

| Operator | Name | Purpose |
|---|---|---|
| `&` | Address-of | Gives the memory address of a variable |
| `*` | Dereference / Indirection | Accesses the value stored at the address a pointer holds |

```c
#include <stdio.h>

int main()
{
    int num = 25;
    int *ptr;          /* declare a pointer to an int */

    ptr = &num;         /* ptr now stores the ADDRESS of num */

    printf("Value of num       : %d\n", num);
    printf("Address of num     : %p\n", &num);
    printf("Value stored in ptr: %p\n", ptr);
    printf("Value pointed to by ptr (*ptr): %d\n", *ptr);

    *ptr = 50;           /* modifies num INDIRECTLY through the pointer */
    printf("num after *ptr=50 : %d\n", num);

    return 0;
}
```

**Sample output** (addresses will vary each run):
```
Value of num       : 25
Address of num     : 0x7ffeeb1a2c3c
Value stored in ptr: 0x7ffeeb1a2c3c
Value pointed to by ptr (*ptr): 25
num after *ptr=50 : 50
```

### Declaring Pointers

```c
int *p;       /* p is a pointer to an int */
float *fp;    /* fp is a pointer to a float */
char *cp;     /* cp is a pointer to a char */
```

The `*` in a declaration means "this variable is a pointer," **not** "dereference" — that dual meaning of `*` (declaration vs. dereference operator) often confuses beginners. Read `int *p;` as "`p` is a pointer to `int`."

### The Two Meanings of `*` — A Critical Distinction

```c
int  x  = 10;
int *p  = &x;   /* HERE: '*' means "p is a pointer to int" (declaration) */
int  y  = *p;   /* HERE: '*' means "dereference p" -- fetch the value it points to */
```

## 9.3 Pointer Types and Their Sizes

- The **size of any pointer variable itself** is fixed by the platform's architecture (typically **8 bytes on a 64-bit system**, 4 bytes on 32-bit) — **regardless of what type it points to**. A pointer just stores an address.
- However, the **type a pointer points to matters enormously** for pointer arithmetic (Chapter 13) and for how many bytes `*ptr` reads/writes.

```c
int i = 10;
char c = 'A';
double d = 3.14;

int    *pi = &i;
char   *pc = &c;
double *pd = &d;

printf("%zu %zu %zu\n", sizeof(pi), sizeof(pc), sizeof(pd));  /* all print the SAME size, e.g., 8 8 8 */
printf("%zu %zu %zu\n", sizeof(*pi), sizeof(*pc), sizeof(*pd)); /* prints 4 1 8 -- the POINTED-TO type's size */
```

## 9.4 Back to Function Calls — Call by Reference

By passing a **pointer** (an address) to a function, the function can directly modify the caller's original variable through dereferencing.

```c
#include <stdio.h>

void doubleIt(int *n)
{
    *n = *n * 2;      /* modifies the ORIGINAL variable via its address */
}

int main()
{
    int x = 5;
    doubleIt(&x);       /* pass the ADDRESS of x */
    printf("x = %d\n", x);   /* now prints 10 */
    return 0;
}
```

### The Classic Swap Function

```c
#include <stdio.h>

void swap(int *a, int *b)
{
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main()
{
    int x = 10, y = 20;
    printf("Before swap: x=%d, y=%d\n", x, y);
    swap(&x, &y);
    printf("After swap : x=%d, y=%d\n", x, y);
    return 0;
}
```

**Output:**
```
Before swap: x=10, y=20
After swap : x=20, y=10
```

> Note: `swap(x, y)` (passing the values, not the addresses) would **not** work — the function would only swap its own local copies, and `main()`'s `x` and `y` would remain unchanged.

## 9.5 Utility of Call by Reference

Call by reference (via pointers) is essential whenever a function must:

1. **Modify multiple variables** in the caller and "return" more than one result (a single `return` statement can send back only one value).
2. **Avoid copying large data** (arrays, structures) for efficiency — pass an address instead of duplicating the whole data.
3. **Allow the caller's variable to genuinely change**, as in `scanf("%d", &x)` — this is exactly why `scanf` requires `&`!

## 9.6 Conclusions

- Pointers, at their core, are just variables that store addresses.
- `&variable` gets an address; `*pointer` dereferences (follows) that address to get/set the value there.
- Passing an address to a function is how C achieves "call by reference," since C itself has no dedicated reference syntax like some other languages.

## 9.7 Uses of Pointers

| Use Case | Chapter |
|---|---|
| Call by reference (modifying caller's variables) | This chapter |
| Efficient array traversal & array-function interaction | Chapter 13, 14 |
| Dynamic memory allocation (`malloc`, `free`) | Chapter 24 (brief mention) |
| String handling (`char *`) | Chapter 15, 16 |
| Building linked data structures (linked lists, trees) | Beyond this course, but foundational here |
| Function pointers (callbacks) | Chapter 22 |

## 9.8 Worked Programs

### Program 1: Pointer Basics

```c
#include <stdio.h>
int main()
{
    int a = 10;
    int *p = &a;

    printf("a = %d\n", a);
    printf("&a = %p\n", &a);
    printf("p = %p\n", p);
    printf("*p = %d\n", *p);

    a = 20;                  /* changing a directly */
    printf("*p after a=20 : %d\n", *p);   /* *p reflects the change immediately -- same memory! */

    return 0;
}
```

### Program 2: Passing an Address to a Function to Modify a Variable

```c
#include <stdio.h>

void increment(int *n)
{
    (*n)++;    /* parentheses required: *n++ would be misread as *(n++) */
}

int main()
{
    int counter = 0, i;
    for (i = 0; i < 5; i++)
        increment(&counter);

    printf("Counter = %d\n", counter);   /* prints 5 */
    return 0;
}
```

> **Precedence trap:** `*n++` is parsed as `*(n++)` (postfix `++` binds tighter than `*`), which increments the *pointer* `n` itself (moving it to point elsewhere), **not** the value it points to! To increment the pointed-to *value*, you must write `(*n)++`.

### Program 3: Finding the Larger of Two Numbers via Pointers

```c
#include <stdio.h>

int larger(int *a, int *b)
{
    return (*a > *b) ? *a : *b;
}

int main()
{
    int x = 15, y = 42;
    printf("Larger = %d\n", larger(&x, &y));
    return 0;
}
```

## 9.9 Common Pitfalls

| Pitfall | Example | Fix |
|---|---|---|
| Uninitialized ("wild") pointer | `int *p; *p = 5;` | Always initialize a pointer (`= &var` or `= NULL`) before dereferencing |
| Dereferencing `NULL` | `int *p = NULL; *p = 5;` | Check `if (p != NULL)` before dereferencing |
| Confusing `*n++` with `(*n)++` | Meant to increment the value, incremented the pointer instead | Use explicit parentheses: `(*n)++` |
| Forgetting `&` when calling a pointer-parameter function | `swap(x, y);` instead of `swap(&x, &y);` | Always pass addresses to functions expecting pointer parameters |
| Mismatched pointer types | `float *fp = &someInt;` | Ensure the pointer's declared type matches the variable it points to |

## 9.10 Key Takeaways

1. A pointer stores the memory address of another variable; `&` obtains an address, `*` dereferences it.
2. `int *p;` declares `p` as "a pointer to `int`" — the `*` here is part of the *type*, not an operation.
3. Passing a pointer to a function (call by reference) lets the function modify the caller's actual variable — impossible with plain call by value.
4. The classic `swap()` function is the canonical illustration of why call by reference matters.
5. Always initialize pointers before use, and be careful with operator precedence when combining `*` with `++`/`--`.
