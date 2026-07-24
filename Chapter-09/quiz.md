# Chapter 9 — Quiz: Pointers

## 📖 Topics Covered: Address-of (`&`), dereference (`*`), call by reference, pointer declarations, swap via pointers

---

## Part A: Multiple Choice Questions (5)

### Q1. Given `int a = 10; int *p = &a;`, what does `*p` represent?

A) The address of `p`
B) The address of `a`
C) The value stored in `a` (i.e., `10`)
D) The size of `a` in bytes

<details>
<summary><b>Answer</b></summary>

**C) The value stored in `a` (i.e., `10`)**

`*p` dereferences the pointer `p`, following the address it stores to access the actual value at that location — which is `a`'s current value, `10`.
</details>

---

### Q2. What is the output of the following program?

```c
#include <stdio.h>
void modify(int *n) { *n = *n + 100; }
int main()
{
    int x = 5;
    modify(&x);
    printf("%d\n", x);
    return 0;
}
```

A) `5`
B) `100`
C) `105`
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**C) `105`**

`&x` passes the address of `x` to `modify`. Inside, `*n` dereferences that address to reach `x` itself, so `*n = *n + 100` actually updates `x` to `5 + 100 = 105`. Since a pointer (address) was passed, the change is visible back in `main()`.
</details>

---

### Q3. What is wrong with the following code?

```c
int *p;
*p = 25;
printf("%d", *p);
```

A) Nothing — this is perfectly valid, safe code
B) `p` is an uninitialized ("wild") pointer being dereferenced, which is undefined behaviour
C) `*p = 25;` should instead be `p = 25;`
D) `printf` cannot print pointer values

<details>
<summary><b>Answer</b></summary>

**B) `p` is an uninitialized ("wild") pointer being dereferenced, which is undefined behaviour**

`p` is declared but never assigned a valid address (e.g., via `&someVariable`). Dereferencing it and writing `25` to whatever random address it happens to hold is undefined behaviour, and can crash the program or silently corrupt unrelated memory.
</details>

---

### Q4. In `void increment(int *n) { *n++; }`, what does `*n++` actually do (compared to what a beginner might expect)?

A) Increments the value pointed to by `n`
B) Increments the pointer `n` itself (moves it to point elsewhere), then dereferences the *old* address — NOT what most beginners intend
C) Causes a compilation error
D) Is exactly equivalent to `(*n)++`

<details>
<summary><b>Answer</b></summary>

**B) Increments the pointer `n` itself (moves it to point elsewhere), then dereferences the *old* address — NOT what most beginners intend**

Postfix `++` binds more tightly than unary `*`, so `*n++` parses as `*(n++)`. This increments the *pointer variable* `n` (moving it forward), and the dereference happens on the original (pre-increment) address; it does **not** increment the pointed-to value. To increment the value itself, you must write `(*n)++`.
</details>

---

### Q5. What is the typical size, in bytes, of an `int *` pointer variable on a 64-bit system, regardless of what it points to?

A) 1 byte
B) 4 bytes
C) 8 bytes
D) It equals `sizeof(int)`, i.e., varies with the pointed-to type

<details>
<summary><b>Answer</b></summary>

**C) 8 bytes**

On a typical 64-bit system, all pointer variables — regardless of the type they point to (`int *`, `char *`, `double *`, etc.) — are the same fixed size (commonly 8 bytes), because a pointer simply stores a memory address, and addresses on that architecture are 64 bits wide. The *pointed-to* type only affects `sizeof(*ptr)`, not `sizeof(ptr)`.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain the two distinct meanings of the `*` symbol in C pointer code, using `int *p = &x; int y = *p;` as your example.

<details>
<summary><b>Model Answer</b></summary>

The `*` symbol has two different roles depending on context:

1. **In a declaration** (`int *p`), `*` is part of the *type* — it declares `p` as "a pointer to `int`," not an `int` itself. No dereferencing happens here; it is purely syntax describing what kind of variable `p` is.
2. **In an expression** (`int y = *p;`), `*` is the **dereference (indirection) operator** — it means "go to the address stored in `p`, and fetch the value found there." Here, `*p` accesses `x`'s current value through `p`.

Beginners often confuse these because the same symbol is reused for two conceptually different purposes; context (declaration vs. expression) always disambiguates it.
</details>

---

### Q2. Why does `swap(int a, int b)` (call by value) fail to actually swap two variables in `main()`, while `swap(int *a, int *b)` (call by reference) succeeds? Illustrate both versions.

<details>
<summary><b>Model Answer</b></summary>

**Call by value (fails):**
```c
void swap(int a, int b)
{
    int temp = a;
    a = b;
    b = temp;   /* only swaps the LOCAL COPIES a and b -- main()'s variables are untouched */
}
```
When `swap(x, y)` is called, `a` and `b` receive *copies* of `x` and `y`'s values. Any changes made to `a`/`b` inside the function only affect those local copies, which are destroyed when the function returns; `main()`'s original `x` and `y` never change.

**Call by reference (succeeds):**
```c
void swap(int *a, int *b)
{
    int temp = *a;
    *a = *b;
    *b = temp;   /* dereferencing means we're directly modifying main()'s x and y */
}
```
Calling `swap(&x, &y)` passes the *addresses* of `x` and `y`. Inside the function, `*a` and `*b` dereference those addresses, directly reading and writing `main()`'s actual `x` and `y` — so the swap is genuinely visible after the function returns.
</details>

---

### Q3. Explain why `scanf("%d", &age);` requires the `&` operator, connecting this to what you have learned about pointers in this chapter.

<details>
<summary><b>Model Answer</b></summary>

`scanf()` needs to **write** a value into the caller's variable `age` — but functions in C normally only receive *copies* of arguments (call by value), so if `scanf` simply received the value of `age`, it would have no way to actually change it in `main()`. By passing `&age` (the *address* of `age`), `scanf` receives a pointer, and can dereference it internally to store the typed-in value directly at that memory location — exactly the same "call by reference" mechanism used by the custom `swap()`/`increment()` functions studied in this chapter. This is precisely why `scanf` needs `&` for scalar variables: it is fundamentally an application of pointers to implement call-by-reference.
</details>

---

### Q4. What is a "wild" or uninitialized pointer? Why is dereferencing one dangerous? Give a safe alternative pattern.

<details>
<summary><b>Model Answer</b></summary>

A **wild (uninitialized) pointer** is a pointer variable that has been declared but not yet assigned a valid address — it contains a garbage/unpredictable value left over in memory, which is *not* a legitimate address of any variable you own.

```c
int *p;        /* wild -- contains garbage, not a valid address */
*p = 25;       /* DANGEROUS: writes to some random, unknown memory location */
```
Dereferencing such a pointer is **undefined behaviour**: it might crash the program (segmentation fault), silently corrupt unrelated data, or — worse — appear to "work" during testing while still being fundamentally broken and prone to failure elsewhere.

**Safe pattern:** Always initialize pointers immediately, either to a valid address or to `NULL` (meaning "points to nothing yet"), and check before dereferencing:
```c
int *p = NULL;
/* ... later ... */
if (p != NULL)
    *p = 25;    /* only dereference after confirming p is valid */
```
</details>

---

### Q5. Given `int i = 5; char c = 'A'; int *pi = &i; char *pc = &c;`, explain why `sizeof(pi)` typically equals `sizeof(pc)` even though `sizeof(i)` does not equal `sizeof(c)`.

<details>
<summary><b>Model Answer</b></summary>

A pointer variable's size depends only on the **width of a memory address** on the target architecture (e.g., 8 bytes on a typical 64-bit system) — this is fixed and identical for *every* pointer type, because all a pointer stores is an address, never the pointed-to data itself. Hence `sizeof(pi) == sizeof(pc)` (both are, say, 8 bytes), regardless of what type they point to.

In contrast, `sizeof(i)` (an `int`, typically 4 bytes) and `sizeof(c)` (a `char`, always 1 byte) reflect the size of the *actual data* stored in those variables, which genuinely differs by type. The pointed-to type only matters when you dereference — `sizeof(*pi)` gives `sizeof(int)` and `sizeof(*pc)` gives `sizeof(char)` — but the pointer variables themselves are uniformly sized.
</details>
