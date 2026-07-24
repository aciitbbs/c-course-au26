# Chapter 6 — Quiz: More Complex Repetitions

## 📖 Topics Covered: `for` loop, nested loops, `break`, `continue`, `do-while`, infinite loops

---

## Part A: Multiple Choice Questions (5)

### Q1. How many times is `"Hi "` printed by the following code?

```c
int i, j;
for (i = 1; i <= 3; i++)
    for (j = 1; j <= 2; j++)
        printf("Hi ");
```

A) 2
B) 3
C) 5
D) 6

<details>
<summary><b>Answer</b></summary>

**D) 6**

The outer loop runs 3 times; for each of those, the inner loop runs 2 times. Total = 3 × 2 = 6.
</details>

---

### Q2. What is the output of the following code?

```c
int i;
for (i = 1; i <= 5; i++)
{
    if (i == 3)
        continue;
    if (i == 5)
        break;
    printf("%d ", i);
}
```

A) `1 2 4`
B) `1 2 3 4`
C) `1 2 4 5`
D) `1 2 3 4 5`

<details>
<summary><b>Answer</b></summary>

**A) `1 2 4`**

`i=1,2`: printed normally. `i=3`: `continue` skips the `printf`, jumping straight to the update (`i++`). `i=4`: printed. `i=5`: `break` exits the loop *before* reaching the `printf`, so `5` is never printed.
</details>

---

### Q3. What distinguishes a `do-while` loop from a `while` loop?

A) `do-while` cannot contain `break`
B) `do-while` checks its condition after executing the body at least once; `while` checks before
C) `do-while` can only be used with integers
D) There is no difference

<details>
<summary><b>Answer</b></summary>

**B) `do-while` checks its condition after executing the body at least once; `while` checks before**

`do-while` is exit-controlled, guaranteeing the loop body runs at least once, even if the condition is false from the start. `while` is entry-controlled and may run zero times.
</details>

---

### Q4. What is the effect of `break` when used inside a loop that is itself nested inside another loop?

A) It exits both loops (inner and outer)
B) It exits only the innermost loop containing it
C) It exits only the outer loop
D) It causes a compilation error

<details>
<summary><b>Answer</b></summary>

**B) It exits only the innermost loop containing it**

`break` always terminates the *nearest enclosing* loop (or `switch`) — not any outer loops. To exit multiple nested loops, you typically need a flag variable or a restructured control flow (e.g., a function with `return`).
</details>

---

### Q5. Which of these is a syntactically valid, deliberately infinite loop in C?

A) `for (;;) { }`
B) `while (0) { }`
C) `for (i=0; i<0; i++) { }`
D) `do { } while (0);`

<details>
<summary><b>Answer</b></summary>

**A) `for (;;) { }`**

Omitting all three parts of a `for` header creates an infinite loop, since there is no condition to ever stop it (equivalent to `while(1)`). Options B, C, and D all have conditions that are false or become false, so their bodies execute zero or exactly one time, not infinitely.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain the exact difference in behaviour between `break` and `continue` inside a loop, using a short example for each.

<details>
<summary><b>Model Answer</b></summary>

- **`break`** immediately terminates the entire nearest enclosing loop; control jumps to the first statement *after* the loop.
  ```c
  for (i = 1; i <= 10; i++)
  {
      if (i == 4) break;
      printf("%d ", i);   /* prints: 1 2 3 */
  }
  ```
- **`continue`** skips only the remaining statements in the *current* iteration and moves on to the next iteration (re-checking the condition, and for `for` loops, running the update expression first).
  ```c
  for (i = 1; i <= 5; i++)
  {
      if (i == 3) continue;
      printf("%d ", i);   /* prints: 1 2 4 5 */
  }
  ```

In short: `break` stops the loop altogether; `continue` only skips ahead within the loop.
</details>

---

### Q2. Why is `do-while` particularly well-suited for menu-driven programs? Illustrate with a short skeleton.

<details>
<summary><b>Model Answer</b></summary>

A menu-driven program needs to **display the menu at least once**, regardless of what the user eventually chooses — the very first display cannot depend on any prior input. Since `do-while` is exit-controlled, its body (displaying the menu and reading a choice) always executes at least once before the exit condition is even checked, which exactly matches this requirement.

```c
int choice;
do
{
    printf("1. Option A\n2. Option B\n0. Exit\n");
    scanf("%d", &choice);
    /* handle choice */
} while (choice != 0);
```

If a `while` loop were used instead, the programmer would need to somehow initialize `choice` to a non-exit value *before* the loop just to force the first pass — an artificial workaround that `do-while` avoids entirely.
</details>

---

### Q3. Convert the following `while` loop into an equivalent `for` loop, and explain the mapping between the two forms.

```c
int i = 2;
while (i <= 20)
{
    printf("%d ", i);
    i = i + 2;
}
```

<details>
<summary><b>Model Answer</b></summary>

```c
int i;
for (i = 2; i <= 20; i = i + 2)
    printf("%d ", i);
```

**Mapping:** The `for` loop's three header components correspond directly to the `while` loop's three separate parts:
- Initialization (`int i = 2;`) becomes the first `for` clause (`i = 2`).
- The loop condition (`i <= 20`) becomes the second `for` clause, unchanged.
- The update step (`i = i + 2;`, previously the last statement in the body) becomes the third `for` clause.

Both loops print `2 4 6 8 10 12 14 16 18 20`.
</details>

---

### Q4. Trace the following nested loop and give its complete output.

```c
int i, j;
for (i = 1; i <= 3; i++)
{
    for (j = 1; j <= i; j++)
        printf("%d", j);
    printf("\n");
}
```

<details>
<summary><b>Model Answer</b></summary>

**Output:**
```
1
12
123
```

Trace: When `i=1`, the inner loop runs for `j=1` only, printing `1`, then a newline. When `i=2`, the inner loop runs `j=1,2`, printing `12`, then a newline. When `i=3`, the inner loop runs `j=1,2,3`, printing `123`, then a newline. This is the classic pattern-printing idiom where the inner loop's upper bound depends on the outer loop's current value.
</details>

---

### Q5. What potential problem exists with the idiom `for (;;) { ... }`, and how is it normally made safe to use?

<details>
<summary><b>Model Answer</b></summary>

`for (;;)` has no condition, so by itself it never terminates — if used carelessly, it produces a genuine infinite loop that hangs the program. It is made safe by including an internal, reachable **`break`** statement guarded by some condition that is checked and can eventually become true during execution:

```c
for (;;)
{
    scanf("%d", &num);
    if (num == -1)   /* sentinel value chosen by the programmer to signal "stop" */
        break;
    printf("You entered: %d\n", num);
}
```

Without a reachable `break` (or an external event like `exit()` or `return`), such a loop would run forever, which is why every intentional infinite loop must be paired with a well-defined internal exit condition.
</details>
