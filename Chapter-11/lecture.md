# Chapter 11 — Lecture: Data Types Revisited

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 11: "Data Types Revisited"

---

## 11.1 Integers — `short`, `long`, `long long`, `signed`, `unsigned`

C's basic `int` type can be modified with size and sign qualifiers to fit different ranges of values and memory budgets:

| Type | Typical Size | Typical Range |
|---|---|---|
| `short int` (`short`) | 2 bytes | -32,768 to 32,767 |
| `int` | 4 bytes | -2,147,483,648 to 2,147,483,647 |
| `long int` (`long`) | 4 or 8 bytes (platform-dependent) | at least -2,147,483,648 to 2,147,483,647 |
| `long long int` (`long long`) | 8 bytes | ~ -9.2×10¹⁸ to 9.2×10¹⁸ |
| `unsigned int` | 4 bytes | 0 to 4,294,967,295 |
| `unsigned long long` | 8 bytes | 0 to ~1.8×10¹⁹ |

```c
#include <stdio.h>
int main()
{
    printf("%zu\n", sizeof(short));       /* typically 2 */
    printf("%zu\n", sizeof(int));         /* typically 4 */
    printf("%zu\n", sizeof(long));        /* 4 or 8, platform-dependent */
    printf("%zu\n", sizeof(long long));   /* typically 8 */
    return 0;
}
```

- **`signed`** (the default for `int`/`char` unless stated otherwise) allows both negative and positive values.
- **`unsigned`** allows only zero and positive values, doubling the maximum positive range for the same storage size, since no bit is reserved for the sign.

```c
unsigned int x = -1;    /* legal but DANGEROUS: wraps around to the LARGEST unsigned value (e.g., 4294967295) */
printf("%u\n", x);
```

> **Format specifiers matter:** use `%u` for `unsigned int`, `%ld`/`%lld` for `long`/`long long`, and `%lu`/`%llu` for their unsigned counterparts. Using the wrong specifier is undefined behaviour and a very common, silent bug.

## 11.2 Chars — `signed`, `unsigned`

`char` may be `signed` (range -128 to 127) or `unsigned` (range 0 to 255) depending on the compiler's default — **plain `char`'s signedness is implementation-defined** in C. If your logic depends on the sign of a `char` (e.g., comparing raw byte values), declare it explicitly as `signed char` or `unsigned char`.

```c
char c = 200;             /* if plain char is signed on this system, this may overflow/wrap unexpectedly */
unsigned char uc = 200;   /* explicitly safe: 0-255 range covers 200 comfortably */
```

## 11.3 Reals — `float`, `double`, `long double`

| Type | Typical Size | Typical Precision |
|---|---|---|
| `float` | 4 bytes | ~6-7 significant decimal digits |
| `double` | 8 bytes | ~15-16 significant decimal digits |
| `long double` | 8, 10, 12, or 16 bytes (platform-dependent) | Extended precision |

> **General guidance:** use `double` by default for real-number computations unless memory is extremely tight — `float`'s reduced precision can accumulate noticeable rounding errors across many operations.

```c
float f = 0.1f + 0.2f;
double d = 0.1 + 0.2;
printf("%.20f\n", f);   /* NOT exactly 0.30000000... -- floating-point representation is inherently approximate */
printf("%.20lf\n", d);
```

## 11.4 A Few More Issues — `sizeof` and Overflow

The `sizeof` operator reports the size, in bytes, of any type or variable, and is invaluable for writing portable code:

```c
printf("%zu\n", sizeof(int));
printf("%zu\n", sizeof(myVariable));
```

**Integer overflow** occurs silently in C (no error is raised) when a computed value exceeds the range representable by its type — the result simply wraps around:

```c
#include <stdio.h>
int main()
{
    int maxInt = 2147483647;    /* INT_MAX on most systems */
    printf("%d\n", maxInt + 1); /* wraps to a large NEGATIVE number: -2147483648 */
    return 0;
}
```

## 11.5 Storage Classes in C

A **storage class** determines a variable's **scope** (where it is visible), **lifetime** (how long it exists in memory), and default **initial value**.

### 11.5.1 Automatic Storage Class (`auto`)

- The **default** storage class for variables declared inside a function/block.
- Scope: local to the block where declared.
- Lifetime: created when the block is entered, destroyed when the block exits.
- Default value: **garbage** (uninitialized).

```c
void f()
{
    auto int x = 10;   /* 'auto' keyword is almost never written explicitly -- it's the default */
    int y = 20;        /* identical to the line above, minus the redundant keyword */
}
```

### 11.5.2 Register Storage Class

- Requests that the compiler try to store the variable in a **CPU register** rather than regular RAM, for faster access — but this is only a *hint*; the compiler may ignore it.
- Scope/lifetime: same as `auto`.
- You cannot take the address (`&`) of a `register` variable in strict standard C, since registers have no memory address.

```c
void loop()
{
    register int i;    /* hint: keep i in a fast CPU register for tight loop performance */
    for (i = 0; i < 1000000; i++)
        ;
}
```

> In modern optimizing compilers, `register` is largely a **historical relic** — compilers are usually better than humans at deciding register allocation automatically, so this keyword is rarely used in modern code.

### 11.5.3 Static Storage Class

- Scope: local to the block (if declared inside a function) — same visibility rules as `auto`.
- Lifetime: **persists for the entire program's execution**, not just while the block is active — its value is **retained** between successive calls to the function.
- Default value: **automatically initialized to 0** (unlike `auto`).
- Initialization with an explicit value happens only **once**, the very first time execution reaches that declaration.

```c
#include <stdio.h>

void counter()
{
    static int count = 0;   /* initialized to 0 ONLY on the very first call */
    count++;
    printf("This function has been called %d time(s)\n", count);
}

int main()
{
    counter();   /* prints: called 1 time(s) */
    counter();   /* prints: called 2 time(s) */
    counter();   /* prints: called 3 time(s) */
    return 0;
}
```

> Without `static`, `count` would be a fresh `auto` variable reset to `0` on every call, and the function would always print `1`.

### 11.5.4 External Storage Class (`extern`)

- Used to **declare** (not define) a global variable that is actually defined in another source file (or elsewhere in the same file), enabling variables to be shared across multiple `.c` files in a multi-file project.
- Scope: the entire program (all files that declare it with `extern`).
- Lifetime: entire program execution.
- Default value: `0` (like `static`, globals are zero-initialized by default).

```c
/* file1.c */
int sharedCounter = 0;    /* actual definition -- allocates storage */

/* file2.c */
extern int sharedCounter; /* declaration only -- refers to file1.c's variable, no new storage allocated */

void increment()
{
    sharedCounter++;
}
```

### 11.5.5 Which to Use When

| Storage Class | Use When |
|---|---|
| `auto` (default) | Ordinary local variables inside functions — the vast majority of variables you write |
| `register` | Rarely needed today; historically for tight-loop counters (compilers optimize this automatically now) |
| `static` (local) | You need a function to "remember" a value across multiple calls (e.g., call counters, running totals) |
| `static` (global) | You want a global variable's visibility restricted to just the current file (encapsulation) |
| `extern` | Sharing one global variable across multiple source files in a multi-file project |

### 11.5.6 A Few Subtle Issues

- A `static` variable inside a function is initialized **exactly once**, on the very first call — subsequent calls skip past the initialization and simply reuse the already-existing, previously-updated value.
- Global variables (declared outside any function) are, by default, given a **`static`-like lifetime automatically** (they exist for the whole program), but are visible to *other files* too unless explicitly marked `static` — marking a global `static` restricts its visibility to just the current file.

## 11.6 Worked Program: Demonstrating `static` vs `auto`

```c
#include <stdio.h>

void demo()
{
    int autoVar = 0;         /* re-initialized to 0 on EVERY call */
    static int staticVar = 0; /* initialized to 0 only on the FIRST call */

    autoVar++;
    staticVar++;

    printf("autoVar = %d, staticVar = %d\n", autoVar, staticVar);
}

int main()
{
    demo();   /* autoVar = 1, staticVar = 1 */
    demo();   /* autoVar = 1, staticVar = 2 */
    demo();   /* autoVar = 1, staticVar = 3 */
    return 0;
}
```

## 11.7 Key Takeaways

1. C offers size/sign variants of `int` (`short`, `long`, `long long`, `signed`, `unsigned`) to match a value's expected range and available memory.
2. Plain `char`'s signedness is implementation-defined; use `signed char`/`unsigned char` explicitly when it matters.
3. `float` gives ~6-7 significant digits of precision; `double` gives ~15-16 — prefer `double` unless memory-constrained.
4. Integer overflow wraps around silently in C; there is no automatic error or warning at runtime.
5. `auto` (default) variables reset every call and hold garbage until initialized; `static` variables persist across calls and default-initialize to zero; `extern` lets variables be shared across multiple files.
