# Chapter 6 — Lecture: More Complex Repetitions

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 6: "More Complex Repetitions"

---

## 6.1 The `for` Loop

The `for` loop packs **initialization, condition, and update** into a single, compact header — making it the most popular loop when the number of iterations is known in advance.

```c
for (initialization; condition; update)
{
    statement(s);
}
```

This is functionally equivalent to:
```c
initialization;
while (condition)
{
    statement(s);
    update;
}
```

```c
#include <stdio.h>
int main()
{
    int i;
    for (i = 1; i <= 5; i++)
        printf("%d ", i);
    printf("\n");
    return 0;
}
```

**Output:** `1 2 3 4 5`

Like `while`, the `for` loop is **entry-controlled** — the condition is tested before each iteration, so the body can execute zero times if the condition is false at the start.

### 6.1.1 Nesting of Loops

A loop written inside another loop's body is a **nested loop** — extremely useful for 2-D patterns, matrices, and tables.

```c
#include <stdio.h>
int main()
{
    int i, j;
    for (i = 1; i <= 3; i++)
    {
        for (j = 1; j <= 3; j++)
            printf("%d ", i * j);
        printf("\n");
    }
    return 0;
}
```

**Output (multiplication table 3×3):**
```
1 2 3
2 4 6
3 6 9
```

> The **inner loop completes all its iterations for each single iteration of the outer loop.** For an outer loop running `m` times and an inner loop running `n` times, the inner statement executes `m × n` times in total.

### 6.1.2 Multiple Initializations in the `for` Loop

The comma operator allows more than one initialization/update expression in a `for` header:

```c
int i, j;
for (i = 0, j = 10; i < j; i++, j--)
    printf("i=%d j=%d\n", i, j);
```

## 6.2 The `break` Statement

`break` immediately **terminates the nearest enclosing loop (or `switch`)**, transferring control to the statement right after it.

```c
#include <stdio.h>
int main()
{
    int i;
    for (i = 1; i <= 10; i++)
    {
        if (i == 6)
            break;          /* exit the loop as soon as i becomes 6 */
        printf("%d ", i);
    }
    printf("\n");
    return 0;
}
```

**Output:** `1 2 3 4 5`

`break` is commonly used to stop searching once a target is found:

```c
int arr[5] = {4, 8, 15, 16, 23};
int target = 15, found = 0, i;
for (i = 0; i < 5; i++)
{
    if (arr[i] == target)
    {
        found = 1;
        break;
    }
}
```

## 6.3 The `continue` Statement

`continue` skips the **rest of the current iteration** and jumps directly to the next iteration's condition check (for `for`, this means the update expression runs next; for `while`/`do-while`, control jumps straight to the condition test).

```c
#include <stdio.h>
int main()
{
    int i;
    for (i = 1; i <= 10; i++)
    {
        if (i % 2 == 0)
            continue;        /* skip printing even numbers */
        printf("%d ", i);
    }
    printf("\n");
    return 0;
}
```

**Output:** `1 3 5 7 9`

> **Important distinction:** `break` **exits** the loop entirely; `continue` only **skips to the next iteration** — the loop keeps running.

## 6.4 The `do-while` Loop

```c
do
{
    statement(s);
} while (condition);   /* semicolon is mandatory here */
```

The `do-while` loop is **exit-controlled**: the condition is checked *after* the body executes, so the body always runs **at least once**, regardless of the condition's initial truth value.

```c
#include <stdio.h>
int main()
{
    int choice;
    do
    {
        printf("\n--- Menu ---\n");
        printf("1. Add\n2. Subtract\n0. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        if (choice == 1)
            printf("You chose Add.\n");
        else if (choice == 2)
            printf("You chose Subtract.\n");
        else if (choice != 0)
            printf("Invalid choice.\n");

    } while (choice != 0);

    printf("Goodbye!\n");
    return 0;
}
```

`do-while` is the natural choice for **menu-driven programs**, where the menu must be displayed at least once, and repeated based on user input.

### `while` vs `for` vs `do-while` — When to Use Which

| Loop | Controlled | Runs body at least once? | Typical use case |
|---|---|---|---|
| `while` | Entry | No | Condition-driven repetition, unknown iteration count |
| `for` | Entry | No | Counter-driven repetition, known iteration count |
| `do-while` | Exit | **Yes** | Menu-driven programs, input validation retries |

## 6.5 The Odd Loop

*Let Us C* highlights a curious but legal C construct: **infinite loops written deliberately**, combined with `break` to control termination from inside:

```c
for (;;)      /* all three parts of the for-header omitted -- an intentional infinite loop */
{
    /* ... */
    if (some_condition)
        break;
}
```

```c
while (1)     /* condition is always non-zero (true) -- infinite loop */
{
    /* ... */
    if (some_condition)
        break;
}
```

Such "infinite loop + `break`" idioms are common when the natural exit condition is more clearly expressed *inside* the loop body than in its header (e.g., in menu-driven programs or when reading input until a sentinel value is seen).

## 6.6 Worked Programs

### Program 1: Sum of Digits Using `for`

```c
#include <stdio.h>
int main()
{
    int n, digitSum = 0;
    printf("Enter a number: ");
    scanf("%d", &n);

    for (; n != 0; n = n / 10)
        digitSum += n % 10;

    printf("Sum of digits = %d\n", digitSum);
    return 0;
}
```

### Program 2: Right-Angled Triangle Pattern (Nested Loop)

```c
#include <stdio.h>
int main()
{
    int rows, i, j;
    printf("Enter number of rows: ");
    scanf("%d", &rows);

    for (i = 1; i <= rows; i++)
    {
        for (j = 1; j <= i; j++)
            printf("* ");
        printf("\n");
    }
    return 0;
}
```

**Sample run (rows = 4):**
```
* 
* * 
* * * 
* * * * 
```

### Program 3: Finding the First 10 Prime Numbers Using `break`/`continue`

```c
#include <stdio.h>
int main()
{
    int num = 2, count = 0, i, isPrime;

    while (count < 10)
    {
        isPrime = 1;
        for (i = 2; i * i <= num; i++)
        {
            if (num % i == 0)
            {
                isPrime = 0;
                break;      /* no need to check further divisors */
            }
        }
        if (!isPrime)
        {
            num++;
            continue;       /* skip to next candidate number */
        }
        printf("%d ", num);
        count++;
        num++;
    }
    printf("\n");
    return 0;
}
```

**Output:** `2 3 5 7 11 13 17 19 23 29`

## 6.7 Common Pitfalls

| Pitfall | Example | Fix |
|---|---|---|
| Missing semicolon after `do-while`'s condition | `} while (x < 10)` | Must be `} while (x < 10);` |
| Confusing `break` with `continue` | Using `break` when you meant to skip just one iteration | `break` exits the loop entirely; `continue` moves to the next iteration |
| Infinite `for(;;)` without a `break` | `for (;;) { printf("hi"); }` | Always include a reachable `break` condition inside |
| `break` inside nested loops only exits the *innermost* loop | Expecting `break` to exit both loops | Use a flag variable, or restructure logic, to exit multiple nested loops |

## 6.8 Key Takeaways

1. `for` is ideal when the number of iterations is known; it bundles init/condition/update in one place.
2. Nested loops execute the inner loop completely for every single iteration of the outer loop.
3. `break` exits the nearest enclosing loop/`switch` entirely; `continue` skips only the current iteration.
4. `do-while` is exit-controlled — guarantees at least one execution — making it perfect for menu-driven programs.
5. `for(;;)` or `while(1)` combined with an internal `break` is a legitimate, common C idiom for loops whose natural termination condition is easier to express mid-body.
