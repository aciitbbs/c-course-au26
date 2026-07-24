# Chapter 8 — Quiz: Functions

## 📖 Topics Covered: Function definition, declaration/prototype, call, parameters, return values, categories of functions

---

## Part A: Multiple Choice Questions (5)

### Q1. What is the correct function prototype for a function `average` that takes two `int` parameters and returns a `float`?

A) `average(int, int);`
B) `float average(int a, int b)`
C) `float average(int a, int b);`
D) `void average(float a, float b);`

<details>
<summary><b>Answer</b></summary>

**C) `float average(int a, int b);`**

A prototype states the return type, function name, and parameter types, ending with a semicolon (no body). Option B is missing the terminating semicolon (that would actually be the start of a definition), option A omits the return type, and option D has the wrong return type and parameter types.
</details>

---

### Q2. Which category of function does the following belong to?

```c
void greet(char name)
{
    printf("Hello!\n");
}
```

A) No arguments, no return value
B) Arguments, no return value
C) No arguments, a return value
D) Arguments and a return value

<details>
<summary><b>Answer</b></summary>

**B) Arguments, no return value**

The function takes a parameter (`name`, though unused in the body) and its return type is `void`, meaning it returns nothing.
</details>

---

### Q3. What happens if you call a function before declaring or defining it, in a modern C compiler like GCC?

A) It always works fine with no issues
B) The compiler assumes it returns `double`
C) The compiler raises an error or a strong warning about an undeclared function
D) The program silently produces wrong output with no diagnostic

<details>
<summary><b>Answer</b></summary>

**C) The compiler raises an error or a strong warning about an undeclared function**

Old, obsolete C standards implicitly assumed an undeclared function returns `int`, but this is dangerous and modern compilers (following C99/C11 rules) flag it as an error or, at minimum, a serious warning. Always declare functions before use.
</details>

---

### Q4. In the function below, what is printed when `max(7, 12)` is called?

```c
int max(int a, int b)
{
    if (a > b)
        return a;
    return b;
    printf("Never printed\n");
}
```

A) `Never printed` followed by `12`
B) Nothing is printed; the function just returns `12` to the caller
C) Compilation error, because code exists after `return`
D) `Never printed` only

<details>
<summary><b>Answer</b></summary>

**B) Nothing is printed; the function just returns `12` to the caller**

`return` immediately exits the function. Since `a > b` (`7 > 12`) is false, the second `return b;` executes and exits the function immediately, so the `printf` after it is unreachable code — it compiles fine (usually with a warning) but never actually runs.
</details>

---

### Q5. Which header must typically be included to use `sqrt()` and `pow()`, and what linker flag is often needed?

A) `<stdlib.h>`, no special flag
B) `<math.h>`, sometimes `-lm`
C) `<string.h>`, `-lstring`
D) `<stdio.h>`, `-lm`

<details>
<summary><b>Answer</b></summary>

**B) `<math.h>`, sometimes `-lm`**

`sqrt`, `pow`, and similar mathematical functions are declared in `<math.h>`. On some systems/compilers, you must also link the math library explicitly with `-lm` when compiling (e.g., `gcc program.c -o program -lm`).
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain the difference between a function prototype (declaration), a function definition, and a function call. Illustrate all three for a function `int square(int n)`.

<details>
<summary><b>Model Answer</b></summary>

- **Prototype/Declaration:** Announces the function's name, return type, and parameter types to the compiler, without providing its body — used so the compiler knows how to correctly check calls made *before* the actual definition appears.
  ```c
  int square(int n);
  ```
- **Definition:** Provides the actual implementation/body of the function.
  ```c
  int square(int n)
  {
      return n * n;
  }
  ```
- **Call:** Invokes the function with actual argument values, at the point where its task is needed.
  ```c
  int result = square(5);
  ```
</details>

---

### Q2. Describe the four possible categories of functions based on arguments and return values, giving a one-line example signature for each.

<details>
<summary><b>Model Answer</b></summary>

1. **No arguments, no return value:** `void showMenu(void);`
2. **Arguments, no return value:** `void printTable(int n);`
3. **No arguments, a return value:** `int getRandomSeed(void);`
4. **Arguments and a return value:** `int add(int a, int b);`

Each combination is equally valid — the correct choice depends purely on whether the task needs external input and/or needs to hand back a computed result.
</details>

---

### Q3. Why are functions considered essential for writing maintainable, "modular" programs? Give at least three distinct benefits.

<details>
<summary><b>Model Answer</b></summary>

1. **Modularity/Decomposition:** A large, complex problem can be broken into smaller, independently understandable sub-tasks, each handled by one function.
2. **Reusability:** A function written once (e.g., `isPrime(int)`) can be called many times from different parts of the program (or even from other programs), avoiding duplicated code.
3. **Readability:** A well-named function call, e.g., `computeGST(amount)`, communicates *intent* clearly, rather than a long inline block of arithmetic that a reader must decode.
4. **Easier testing/debugging:** Small, focused functions can be tested in isolation, making it far easier to locate the source of a bug than searching through one giant `main()`.

(Any three well-explained points suffice for full credit.)
</details>

---

### Q4. Trace the following program and explain, step by step, what value is returned by `isPrime(9)` and why.

```c
int isPrime(int n)
{
    int i;
    if (n <= 1)
        return 0;
    for (i = 2; i * i <= n; i++)
    {
        if (n % i == 0)
            return 0;
    }
    return 1;
}
```

<details>
<summary><b>Model Answer</b></summary>

For `n = 9`: `n <= 1` is false, so we proceed to the loop. `i` starts at `2`; the loop condition is `i*i <= 9`, i.e., `4 <= 9`, true. Check `9 % 2 == 0`? No (`9 % 2 = 1`), so continue. `i` becomes `3`; condition `3*3 <= 9`, i.e., `9 <= 9`, true. Check `9 % 3 == 0`? **Yes** (`9 % 3 = 0`), so the function immediately executes `return 0;`.

**Result:** `isPrime(9)` returns `0` (false), correctly identifying that 9 is not prime (since 9 = 3 × 3).
</details>

---

### Q5. What is the "one dicey issue" the textbook warns about regarding calling a function before it is declared? Explain the risk and the fix.

<details>
<summary><b>Model Answer</b></summary>

If a function is *called* in the source code before the compiler has seen either its full definition or at least a prototype declaration, older/obsolete C rules would silently assume the function returns a plain `int` and accept any arguments without type-checking them — a dangerous, error-hiding default, because a mismatch between the assumed and actual signature (e.g., the real function actually returns `float` or expects different argument types) can produce corrupted results or undefined behaviour without any compiler warning under those old rules.

Modern compilers (adhering to C99/C11) instead treat this situation as an error or a serious warning, refusing to guess. The fix is always to **declare a prototype for every function before it is first used** (commonly done by placing all prototypes near the top of the file, just after the `#include` directives), or to physically place the function's full definition before `main()` (or before any other function that calls it).
</details>
