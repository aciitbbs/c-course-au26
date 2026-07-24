# Chapter 5 — Quiz: Loop Control Instruction

## 📖 Topics Covered: `while` loop, entry-controlled loops, increment/decrement operators, common loop bugs

---

## Part A: Multiple Choice Questions (5)

### Q1. How many times does the loop body execute in the following code?

```c
int i = 10;
while (i < 5)
{
    printf("%d ", i);
    i++;
}
```

A) 0 times
B) 1 time
C) 5 times
D) Infinite times

<details>
<summary><b>Answer</b></summary>

**A) 0 times**

`while` is entry-controlled: the condition `i < 5` is checked *before* the first iteration. Since `i` starts at `10`, the condition is false immediately, so the body never executes.
</details>

---

### Q2. What is the effect of the following code?

```c
int i = 1;
while (i <= 5);
{
    printf("%d ", i);
    i++;
}
```

A) Prints `1 2 3 4 5`
B) Prints `1` once
C) Infinite loop that produces no output
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**C) Infinite loop that produces no output**

The semicolon immediately after `while (i <= 5)` makes the loop's body an empty statement. Since `i` is never modified inside this empty body, the condition `i <= 5` remains true forever, and the program hangs producing no output at all (the block below is unrelated, dead code that never executes).
</details>

---

### Q3. What does the following code print?

```c
int i = 5;
printf("%d %d\n", i++, i);
```

A) `5 5`
B) `5 6`
C) `6 6`
D) Undefined / compiler-dependent behaviour

<details>
<summary><b>Answer</b></summary>

**D) Undefined / compiler-dependent behaviour**

Modifying `i` (via `i++`) and reading `i` again within the same, unsequenced function-argument list is undefined behaviour in C — the order in which arguments to `printf` are evaluated is not specified by the standard. Different compilers may print different results. (Best practice: never rely on the order of side effects within the same statement; always avoid such expressions in real code.)
</details>

---

### Q4. What is the value of `x` after the following?

```c
int i = 5, x;
x = ++i;
```

A) `4`
B) `5`
C) `6`
D) `x` is undefined

<details>
<summary><b>Answer</b></summary>

**C) `6`**

`++i` is pre-increment: `i` is incremented to `6` *first*, and then that new value (`6`) is used in the assignment, so `x` becomes `6`.
</details>

---

### Q5. In the prime-checking loop `while (i * i <= n && isPrime == 1)`, why is `i * i <= n` used instead of a direct call to compute the square root?

A) It is required syntax in C
B) It avoids needing `<math.h>` and floating-point comparisons, while achieving the same effect
C) `sqrt()` does not exist in C
D) It makes the loop run faster by skipping iterations incorrectly

<details>
<summary><b>Answer</b></summary>

**B) It avoids needing `<math.h>` and floating-point comparisons, while achieving the same effect**

`i * i <= n` is algebraically equivalent to `i <= sqrt(n)` but uses only integer arithmetic, avoiding the need to `#include <math.h>`, link the math library, or deal with floating-point rounding — a common competitive-programming idiom.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. What does it mean for the `while` loop to be "entry-controlled"? Contrast this conceptually with a loop that would test its condition *after* the body executes.

<details>
<summary><b>Model Answer</b></summary>

"Entry-controlled" means the loop's condition is evaluated **before** each iteration begins, including before the very first one. As a result, if the condition is false at the outset, the loop body may execute **zero times**.

This contrasts with an *exit-controlled* loop (introduced in the next chapter as `do-while`), which checks its condition **after** executing the body, guaranteeing the body runs **at least once**, even if the condition turns out to be false immediately.

```c
int i = 10;
while (i < 5)          /* entry-controlled: body never runs, since 10 < 5 is false immediately */
    printf("%d", i);
```
</details>

---

### Q2. Explain the bug in the following code and rewrite it correctly. What symptom would a student observe when running this program?

```c
int i = 1;
while (i <= 10)
{
    printf("%d ", i);
}
```

<details>
<summary><b>Model Answer</b></summary>

**Bug:** The loop control variable `i` is never updated inside the loop body — there is no `i++` (or similar). Since `i` always stays `1`, the condition `i <= 10` is always true, so the loop **never terminates** — an infinite loop.

**Symptom:** The program will appear to "hang" — it keeps printing `1 1 1 1 1 ...` forever without ever returning control, until manually terminated (e.g., Ctrl+C).

**Corrected code:**
```c
int i = 1;
while (i <= 10)
{
    printf("%d ", i);
    i++;   /* ensures the loop eventually terminates */
}
```
</details>

---

### Q3. Differentiate between pre-increment (`++i`) and post-increment (`i++`) with a worked example showing different results when the value is used immediately.

<details>
<summary><b>Model Answer</b></summary>

Both `++i` and `i++` ultimately increase `i` by `1`, but they differ in **what value the expression itself yields** when used within a larger expression:

- **Post-increment (`i++`):** yields the *original* value of `i`, and *then* increments `i`.
- **Pre-increment (`++i`):** increments `i` *first*, and yields the *new* value.

```c
int i = 5;
int a = i++;   /* a = 5 (original value); i becomes 6 afterward */

int j = 5;
int b = ++j;   /* j becomes 6 first; b = 6 (new value) */
```

If the result of the increment expression is not used immediately (e.g., `i++;` as a standalone statement in a loop), both forms have the exact same net effect on `i`.
</details>

---

### Q4. Trace the following program by hand for the input `1234` and show the value of `n`, `digit`, and `reversed` after each iteration.

```c
int n = 1234, reversed = 0, digit;
while (n != 0)
{
    digit = n % 10;
    reversed = reversed * 10 + digit;
    n = n / 10;
}
```

<details>
<summary><b>Model Answer</b></summary>

| Iteration | `n` (before) | `digit` | `reversed` (after) | `n` (after `n/10`) |
|---|---|---|---|---|
| 1 | 1234 | 4 | 0×10+4 = 4 | 123 |
| 2 | 123 | 3 | 4×10+3 = 43 | 12 |
| 3 | 12 | 2 | 43×10+2 = 432 | 1 |
| 4 | 1 | 1 | 432×10+1 = 4321 | 0 |

The loop stops when `n == 0`. Final answer: `reversed = 4321`.
</details>

---

### Q5. Identify and explain two distinct common bugs that can occur when writing a `while` loop, other than forgetting to update the control variable.

<details>
<summary><b>Model Answer</b></summary>

1. **Accidental empty body from a stray semicolon:**
   ```c
   while (i <= 5);   /* the ';' here IS the entire loop body -- an empty statement */
   {
       printf("%d", i);   /* this block is NOT part of the loop; runs once after the loop (eventually) */
       i++;
   }
   ```
   Because the loop body is empty and does nothing to change `i`, this typically produces an infinite loop that hangs the program with no visible output.

2. **Off-by-one error in the condition (`<` vs `<=`):**
   ```c
   int i = 1;
   while (i < 5)     /* intended to print 1 through 5, but stops after printing 1 2 3 4 */
       { printf("%d ", i); i++; }
   ```
   Using `<` instead of the intended `<=` causes the loop to execute one iteration fewer than expected — a classic "off-by-one" bug that requires careful dry-running of boundary values to catch.
</details>
