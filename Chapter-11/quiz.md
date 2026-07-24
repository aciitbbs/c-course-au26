# Chapter 11 — Quiz: Data Types Revisited

## 📖 Topics Covered: Integer/char/real type variants, `sizeof`, overflow, storage classes (`auto`, `register`, `static`, `extern`)

---

## Part A: Multiple Choice Questions (5)

### Q1. What is the output of the following program?

```c
#include <stdio.h>
void counter()
{
    static int count = 0;
    count++;
    printf("%d ", count);
}
int main()
{
    counter();
    counter();
    counter();
    return 0;
}
```

A) `1 1 1`
B) `1 2 3`
C) `0 1 2`
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**B) `1 2 3`**

`static` variables are initialized only once (the first time execution reaches that line) and retain their value across successive calls to the function, so `count` keeps incrementing from its previous value on each call.
</details>

---

### Q2. What is the default storage class for a variable declared inside a function without any explicit keyword?

A) `static`
B) `extern`
C) `auto`
D) `register`

<details>
<summary><b>Answer</b></summary>

**C) `auto`**

Local variables are `auto` by default; this keyword is almost never written explicitly since it is the assumed default.
</details>

---

### Q3. What happens when `int maxInt = 2147483647; printf("%d", maxInt + 1);` is executed on a typical 32-bit `int` system?

A) The program crashes with an overflow error
B) It prints `2147483648`
C) It silently wraps around to a large negative number (e.g., `-2147483648`)
D) The compiler refuses to compile this code

<details>
<summary><b>Answer</b></summary>

**C) It silently wraps around to a large negative number (e.g., `-2147483648`)**

Signed integer overflow in C does not raise a runtime error by default; the value wraps around according to the underlying binary representation, often producing a large negative number — a subtle and dangerous class of bug.
</details>

---

### Q4. Why can plain `char`'s signedness cause portability issues?

A) `char` cannot hold negative numbers on any platform
B) Whether plain `char` is signed or unsigned by default is implementation-defined, varying by compiler/platform
C) `char` is always 2 bytes
D) `char` cannot be used in arithmetic expressions

<details>
<summary><b>Answer</b></summary>

**B) Whether plain `char` is signed or unsigned by default is implementation-defined, varying by compiler/platform**

The C standard leaves it up to each implementation whether plain `char` behaves as `signed char` or `unsigned char`. Code that depends on this sign behaviour should explicitly declare `signed char` or `unsigned char` to guarantee portable behaviour.
</details>

---

### Q5. Which storage class is most appropriate for a variable in a `.c` file that must be visible and shared with other `.c` files in the same multi-file project?

A) `auto` in the file that uses it
B) `register` in every file
C) A single definition (e.g., `int x;`) in one file, and `extern int x;` declarations in the others
D) `static` in every file

<details>
<summary><b>Answer</b></summary>

**C) A single definition (e.g., `int x;`) in one file, and `extern int x;` declarations in the others**

`extern` declares (without allocating new storage) a reference to a variable actually defined once elsewhere, enabling it to be shared safely across multiple translation units/files. `static` would instead restrict, not enable, cross-file visibility.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain the difference between `auto` and `static` local variables, focusing on both their default initial value and their lifetime across multiple function calls.

<details>
<summary><b>Model Answer</b></summary>

An **`auto`** (the default) local variable is created fresh every time the function/block is entered and destroyed when it exits; it has **no automatic default value** (it holds garbage until explicitly initialized), and any value it held is lost once the function returns — the next call starts over from scratch.

A **`static`** local variable, in contrast, is created once and **persists for the entire program's lifetime**, even though its *scope* (visibility) is still limited to the block where it is declared. It is **automatically initialized to `0`** by default if no explicit initializer is given, and any explicit initializer (`static int x = 5;`) executes only **once**, on the very first time control reaches that statement — on every later call, the previously updated value is retained and reused, rather than reset.
</details>

---

### Q2. What is integer overflow, and why is it considered dangerous in C? Give a concrete example.

<details>
<summary><b>Model Answer</b></summary>

**Integer overflow** occurs when an arithmetic result exceeds the maximum (or falls below the minimum) value representable by a variable's data type. In C, this does **not** raise any runtime error or exception by default — the value simply "wraps around" based on the underlying binary representation, silently producing an incorrect (often drastically wrong, even negative) result.

```c
int x = 2147483647;      /* INT_MAX on a typical 32-bit int */
x = x + 1;                /* silently wraps to -2147483648, NOT 2147483648 */
```

This is dangerous because such bugs can go completely unnoticed during testing (if boundary values aren't specifically tested) and can cause serious, hard-to-diagnose logic errors or even security vulnerabilities in real-world software (e.g., a wrapped-around size calculation leading to a buffer overflow).
</details>

---

### Q3. Explain what the `sizeof` operator does, and give three practical reasons a programmer would use it.

<details>
<summary><b>Model Answer</b></summary>

`sizeof` is a compile-time operator that returns the number of bytes occupied by a given type or variable, e.g., `sizeof(int)`, `sizeof(myArray)`.

**Practical uses:**
1. **Portability** — sizes of `int`, `long`, pointers, etc. can differ across platforms/compilers; using `sizeof` instead of hardcoded numbers keeps code correct everywhere.
2. **Dynamic memory allocation** — functions like `malloc(n * sizeof(int))` (covered later) need the exact byte size of the type being allocated.
3. **Determining array length** — `sizeof(array) / sizeof(array[0])` is the standard idiom for computing the number of elements in a statically-declared array (useful before array-length parameters are passed explicitly).
</details>

---

### Q4. Why is `register` considered mostly a "historical relic" in modern C programming? What does it actually request from the compiler, and why can the compiler ignore it?

<details>
<summary><b>Model Answer</b></summary>

`register` is a **hint** to the compiler, suggesting that a variable should be stored in a CPU register (extremely fast to access) rather than ordinary RAM, useful historically for performance-critical loop counters. However, the C standard explicitly allows the compiler to **ignore** this hint entirely — it is not a binding instruction.

Modern optimizing compilers perform sophisticated register-allocation analysis automatically, generally making far better decisions about which variables deserve registers than a programmer's manual hints could. As a result, `register` rarely has any measurable effect on modern code and is now considered largely a historical remnant from earlier, less-optimizing compilers — though it remains valid, legal syntax.
</details>

---

### Q5. A programmer writes a function to count how many times it has been called, but mistakenly uses `auto` (the default) instead of `static` for the counter variable. Explain the resulting bug and show the fix.

<details>
<summary><b>Model Answer</b></summary>

**Buggy code:**
```c
void counter()
{
    int count = 0;   /* auto by default -- re-initialized to 0 on EVERY call */
    count++;
    printf("%d\n", count);
}
```
Because `count` is an ordinary `auto` local variable, it is destroyed and recreated (reset to `0`) every single time `counter()` is called. So every call prints `1`, never accumulating across calls — the intended "running total of calls" behaviour is completely lost.

**Fix — use `static`:**
```c
void counter()
{
    static int count = 0;   /* initialized to 0 only ONCE, on the first call */
    count++;
    printf("%d\n", count);
}
```
Now `count` persists between calls, correctly printing `1, 2, 3, ...` on successive invocations, because `static` variables retain their value across the entire program's execution rather than being reset each time.
</details>
