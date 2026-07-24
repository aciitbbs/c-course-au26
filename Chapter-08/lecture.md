# Chapter 8 — Lecture: Functions

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 8: "Functions"

---

## 8.1 What is a Function?

A **function** is a self-contained block of code designed to perform a specific, well-defined task. Every C program you have written so far has used at least one function: `main()`. As programs grow, splitting logic into multiple functions makes code more organized, reusable, and testable.

```c
return_type function_name(parameter_list)
{
    /* body */
    return value;   /* omit if return_type is void */
}
```

```c
#include <stdio.h>

int square(int n)          /* function definition */
{
    return n * n;
}

int main()
{
    int result = square(5);   /* function call */
    printf("Square = %d\n", result);
    return 0;
}
```

**Output:** `Square = 25`

### 8.1.1 Why Use Functions?

| Benefit | Explanation |
|---|---|
| **Modularity** | Break a large problem into smaller, manageable pieces |
| **Reusability** | Write once, call from many places (even other programs, via libraries) |
| **Readability** | `isPrime(n)` communicates intent far better than inline logic repeated everywhere |
| **Easier debugging/testing** | Test small functions independently before integrating |
| **Abstraction** | The caller doesn't need to know *how* a function works internally, only *what* it does |

## 8.2 Communication Between Functions

Functions communicate through **parameters** (inputs) and a **return value** (output). Based on this, functions fall into four categories:

| Category | Example |
|---|---|
| No arguments, no return value | `void printBanner(void)` |
| Arguments, no return value | `void printSquare(int n)` |
| No arguments, a return value | `int getRandomNumber(void)` |
| Arguments and a return value | `int add(int a, int b)` |

```c
#include <stdio.h>

/* No arguments, no return value */
void printBanner(void)
{
    printf("=== Welcome ===\n");
}

/* Arguments, no return value */
void printSquare(int n)
{
    printf("Square of %d is %d\n", n, n * n);
}

/* No arguments, a return value */
int getFixedValue(void)
{
    return 42;
}

/* Arguments and a return value */
int add(int a, int b)
{
    return a + b;
}

int main()
{
    printBanner();
    printSquare(6);
    printf("Fixed value = %d\n", getFixedValue());
    printf("Sum = %d\n", add(3, 4));
    return 0;
}
```

### Function Prototype (Declaration) vs Definition vs Call

- **Prototype/Declaration:** Tells the compiler the function's name, return type, and parameter types *before* it is used — needed if the function is defined *after* `main()`.
  ```c
  int add(int a, int b);   /* prototype -- ends with a semicolon */
  ```
- **Definition:** The actual body/implementation of the function.
  ```c
  int add(int a, int b)
  {
      return a + b;
  }
  ```
- **Call:** Invoking the function to execute it.
  ```c
  int result = add(3, 4);   /* call */
  ```

```c
#include <stdio.h>

int add(int a, int b);      /* prototype: lets main() call add() before its definition appears */

int main()
{
    printf("Sum = %d\n", add(3, 4));   /* call */
    return 0;
}

int add(int a, int b)        /* definition */
{
    return a + b;
}
```

> If a function is **defined before `main()`** in the source file, no separate prototype is strictly required (the definition itself serves as the declaration). Placing prototypes at the top of the file — regardless of definition order — is still considered good practice for readability.

## 8.3 Order of Passing Arguments

C evaluates function call arguments, but the **order in which arguments themselves are evaluated is not guaranteed** by the standard (similar to the caution mentioned for `printf` in earlier chapters). Avoid writing code that depends on a specific evaluation order of arguments with side effects.

```c
int i = 5;
foo(i++, i++);   /* AVOID: unspecified which i++ is evaluated first */
```

Parameters, however, are always **matched positionally** — the first argument in the call maps to the first parameter in the definition, and so on.

## 8.4 Using Library Functions

C provides a rich set of ready-made functions in its **standard library**, declared in header files:

| Header | Example functions |
|---|---|
| `<stdio.h>` | `printf`, `scanf`, `getchar`, `putchar` |
| `<math.h>` | `sqrt`, `pow`, `abs` (careful: use `<stdlib.h>` for integer `abs`), `floor`, `ceil` |
| `<string.h>` | `strlen`, `strcpy`, `strcmp` (Chapter 15) |
| `<stdlib.h>` | `malloc`, `free`, `rand`, `exit` |

```c
#include <stdio.h>
#include <math.h>

int main()
{
    double x = 2.0;
    printf("sqrt(2) = %.4f\n", sqrt(x));
    printf("2^10   = %.0f\n", pow(2, 10));
    return 0;
}
```

> When using `<math.h>` functions, some older compilers/linkers require the `-lm` flag: `gcc program.c -o program -lm`. Modern GCC often links it automatically, but it's good practice to include it.

## 8.5 "One Dicey Issue" — Functions Used Before Declaration

If you call a function **before** either its definition or a prototype has been seen by the compiler, older C standards would implicitly assume it returns `int` — a dangerous, error-prone default. Modern compilers (C99 and later, and GCC with default settings) will instead raise an **error or a strong warning**. Always declare (via prototype) or define every function **before** its first use, without exception.

```c
#include <stdio.h>
int main()
{
    printf("%.2f\n", average(4, 5));   /* ERROR/warning in modern GCC: 'average' not declared */
    return 0;
}

float average(int a, int b)
{
    return (a + b) / 2.0f;
}
```

**Fix:** add a prototype `float average(int a, int b);` before `main()`, or move the definition above `main()`.

## 8.6 Return Type of a Function

- If a function does not return a value, its return type is declared **`void`**.
- `main()` conventionally returns `int`: `0` for success, non-zero for an error condition, reported to the operating system/shell.
- A function can only return **one value** directly (multiple "return values" require pointers/structures — covered in later chapters).
- The `return` statement immediately exits the function; any code after an executed `return` in the same control path never runs.

```c
int max(int a, int b)
{
    if (a > b)
        return a;    /* function exits here if a > b */
    return b;        /* only reached if a <= b */
}
```

## 8.7 Worked Programs

### Program 1: Factorial Using a Function

```c
#include <stdio.h>

long factorial(int n)
{
    long fact = 1;
    int i;
    for (i = 1; i <= n; i++)
        fact = fact * i;
    return fact;
}

int main()
{
    int n;
    printf("Enter n: ");
    scanf("%d", &n);
    printf("%d! = %ld\n", n, factorial(n));
    return 0;
}
```

### Program 2: Checking Prime Using a Boolean-Style Function

```c
#include <stdio.h>

int isPrime(int n)
{
    int i;
    if (n <= 1)
        return 0;    /* 0 represents "false" */
    for (i = 2; i * i <= n; i++)
    {
        if (n % i == 0)
            return 0;
    }
    return 1;        /* 1 represents "true" */
}

int main()
{
    int n;
    printf("Enter n: ");
    scanf("%d", &n);

    if (isPrime(n))
        printf("%d is Prime.\n", n);
    else
        printf("%d is NOT Prime.\n", n);

    return 0;
}
```

### Program 3: Menu-Driven Program Using Multiple Functions

```c
#include <stdio.h>

int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }

int main()
{
    int a, b, choice;

    printf("Enter two integers: ");
    scanf("%d %d", &a, &b);

    printf("1. Add  2. Subtract  3. Multiply\nChoice: ");
    scanf("%d", &choice);

    switch (choice)
    {
        case 1: printf("Result = %d\n", add(a, b)); break;
        case 2: printf("Result = %d\n", subtract(a, b)); break;
        case 3: printf("Result = %d\n", multiply(a, b)); break;
        default: printf("Invalid choice.\n");
    }
    return 0;
}
```

## 8.8 Key Takeaways

1. A function packages a task into a reusable, named unit with optional parameters and an optional return value.
2. Functions fall into four combinations of (arguments? / return value?), all equally valid depending on the task.
3. A prototype tells the compiler about a function's signature ahead of its actual definition/use.
4. Every function must be declared (via prototype or definition) *before* it is called — modern compilers enforce this strictly.
5. `return` immediately exits the function, optionally carrying back a single value of the declared return type; use `void` if there is nothing to return.
