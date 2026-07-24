# Chapter 5 — Lecture: Loop Control Instruction

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 5: "Loop Control Instruction"

---

## 5.1 Loops — Why We Need Them

A **loop** repeats a group of statements until a specified condition is satisfied. Without loops, printing "Hello" 100 times would require writing 100 `printf` statements! Loops let us express *repetition* concisely.

Three ingredients are common to every loop:

1. **Initialization** — setting up a counter/control variable before the loop begins.
2. **Condition** — a test evaluated before (or after) each iteration; the loop continues while it is true.
3. **Update** — a step (usually incrementing/decrementing the counter) that eventually makes the condition false, so the loop terminates.

## 5.2 The `while` Loop

```c
while (condition)
{
    statement(s);
}
```

- The `while` loop is **entry-controlled**: the condition is tested *before* each iteration, so the body may execute **zero** times if the condition is false at the very first check.

```c
#include <stdio.h>
int main()
{
    int i = 1;
    while (i <= 5)
    {
        printf("%d ", i);
        i++;
    }
    printf("\n");
    return 0;
}
```

**Output:** `1 2 3 4 5`

### 5.2.1 Tips and Traps

**Trap 1 — Infinite loop from forgetting to update the counter:**
```c
int i = 1;
while (i <= 5)
{
    printf("%d ", i);
    /* forgot i++ -- this loop NEVER terminates! */
}
```

**Trap 2 — Accidental semicolon:**
```c
int i = 1;
while (i <= 5);   /* <-- semicolon here makes the loop body EMPTY */
{
    printf("%d ", i);   /* this block runs exactly once, AFTER the (infinite) empty loop */
    i++;
}
```
Since the body is empty (`;`) and `i` is never updated inside it, this creates an **infinite loop** that does nothing (a classic hang).

**Trap 3 — Off-by-one errors:** Carefully decide `<` vs `<=` in your condition; this is the single most common source of loops running one time too many or too few.

### 5.2.2 More Operators — Increment/Decrement

| Operator | Meaning | Example (`i=5`) |
|---|---|---|
| `i++` | post-increment: use `i`, *then* increment | `x = i++;` → `x=5`, `i` becomes `6` |
| `++i` | pre-increment: increment, *then* use `i` | `x = ++i;` → `i` becomes `6`, `x=6` |
| `i--` | post-decrement | `x = i--;` → `x=5`, `i` becomes `4` |
| `--i` | pre-decrement | `x = --i;` → `i` becomes `4`, `x=4` |

```c
int i = 5;
printf("%d\n", i++);   /* prints 5, then i becomes 6 */
printf("%d\n", i);     /* prints 6 */

int j = 5;
printf("%d\n", ++j);   /* j becomes 6 first, then prints 6 */
```

Compound assignment operators are also commonly used inside loops: `+=`, `-=`, `*=`, `/=`, `%=` — e.g., `sum += i;` is shorthand for `sum = sum + i;`.

## 5.3 Worked Programs with `while`

### Program 1: Sum of First N Natural Numbers

```c
#include <stdio.h>
int main()
{
    int n, i = 1, sum = 0;
    printf("Enter n: ");
    scanf("%d", &n);

    while (i <= n)
    {
        sum = sum + i;
        i++;
    }
    printf("Sum = %d\n", sum);
    return 0;
}
```

### Program 2: Reversing the Digits of a Number

```c
#include <stdio.h>
int main()
{
    int n, reversed = 0, digit;
    printf("Enter a number: ");
    scanf("%d", &n);

    while (n != 0)
    {
        digit = n % 10;
        reversed = reversed * 10 + digit;
        n = n / 10;
    }
    printf("Reversed number = %d\n", reversed);
    return 0;
}
```

**Sample run:** Input `1234` → Output `Reversed number = 4321`

### Program 3: Checking if a Number is Prime

```c
#include <stdio.h>
int main()
{
    int n, i = 2, isPrime = 1;
    printf("Enter a positive integer: ");
    scanf("%d", &n);

    if (n <= 1)
        isPrime = 0;

    while (i * i <= n && isPrime == 1)
    {
        if (n % i == 0)
            isPrime = 0;
        i++;
    }

    if (isPrime)
        printf("%d is Prime.\n", n);
    else
        printf("%d is NOT Prime.\n", n);

    return 0;
}
```

> Notice the efficiency trick: we only need to test divisors `i` up to `sqrt(n)` (expressed here as `i * i <= n` to avoid needing `<math.h>`), because if `n` has a factor larger than its square root, it must also have a corresponding factor smaller than the square root.

## 5.4 Key Takeaways

1. `while` is entry-controlled: the condition is checked *before* the body runs, so the body can execute zero times.
2. Always ensure the loop's controlling variable is updated inside the body — otherwise you get an infinite loop.
3. A stray semicolon right after `while (condition)` silently creates an empty loop body — watch for this bug carefully.
4. `++`/`--` come in prefix and postfix forms with different evaluation-order semantics; be careful when they appear inside larger expressions.
5. Off-by-one mistakes (`<` vs `<=`) are the most frequent loop bugs — always dry-run boundary values.

---

*(Chapter 6, "More Complex Repetitions", continues with the `for` loop, `do-while` loop, nested loops, `break`, and `continue`.)*
