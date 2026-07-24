# Chapter 3 — Lecture: Decision Control Instruction

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 3: "Decision Control Instruction"

---

## 3.1 Why Decision Control?

So far, our programs have executed instructions strictly one after another (**sequential execution**). Real-world logic, however, often needs to choose between alternative actions based on a condition — e.g., "if the marks are above 40, print PASS, otherwise print FAIL." C provides the **`if`** and **`if-else`** statements for exactly this purpose.

## 3.2 The `if` Statement

```c
if (this condition is true)
    execute this statement;
```

- The condition is any expression that evaluates to **zero (false)** or **non-zero (true)**.
- If the condition is true (non-zero), the statement immediately following `if` executes; otherwise, it is skipped.

```c
#include <stdio.h>
int main()
{
    int marks;
    printf("Enter marks: ");
    scanf("%d", &marks);

    if (marks >= 40)
        printf("You have passed.\n");

    printf("Thank you.\n");   /* always executes */
    return 0;
}
```

## 3.3 The `if-else` Statement

```c
if (condition)
    statement-block-1;   /* executed if condition is true */
else
    statement-block-2;   /* executed if condition is false */
```

```c
#include <stdio.h>
int main()
{
    int marks;
    printf("Enter marks: ");
    scanf("%d", &marks);

    if (marks >= 40)
        printf("Result: PASS\n");
    else
        printf("Result: FAIL\n");

    return 0;
}
```

### Relational and Equality Operators

| Operator | Meaning |
|---|---|
| `<` | less than |
| `<=` | less than or equal to |
| `>` | greater than |
| `>=` | greater than or equal to |
| `==` | equal to |
| `!=` | not equal to |

> **The `=` vs `==` trap:** `if (marks = 40)` is a valid C statement — it *assigns* `40` to `marks` and the condition evaluates to `40` (non-zero → always true)! You almost certainly meant `if (marks == 40)`. This is one of the most common C bugs and a favourite exam trick question.

## 3.4 Multiple Statements within `if-else`

If more than one statement must execute conditionally, they must be grouped inside **braces `{ }`** to form a **compound statement (block)**. Without braces, `if`/`else` binds to only the *single* statement immediately following it.

```c
if (marks >= 40)
{
    printf("Result: PASS\n");
    printf("Congratulations!\n");
}
else
{
    printf("Result: FAIL\n");
    printf("Please reappear for the exam.\n");
}
```

**Common bug — missing braces:**
```c
if (marks >= 40)
    printf("Result: PASS\n");
    printf("Congratulations!\n");   /* NOT part of the if! Always executes. */
```
Here, only the first `printf` is controlled by the `if`; the second runs unconditionally regardless of indentation, because indentation has **no syntactic meaning** in C.

## 3.5 Nested `if-else`s

An `if` or `else` block may itself contain another complete `if-else` — this is called **nesting**.

```c
#include <stdio.h>
int main()
{
    int age;
    char hasLicense;

    printf("Enter age: ");
    scanf("%d", &age);

    if (age >= 18)
    {
        printf("Do you have a driving license (y/n)? ");
        scanf(" %c", &hasLicense);

        if (hasLicense == 'y' || hasLicense == 'Y')
            printf("You may drive.\n");
        else
            printf("Please obtain a license first.\n");
    }
    else
    {
        printf("You are not eligible to drive yet.\n");
    }

    return 0;
}
```

### The Dangling `else` Problem

```c
if (a > b)
    if (a > c)
        printf("a is the largest\n");
else
    printf("c is the largest\n");   /* This 'else' binds to the INNER 'if (a > c)', not the outer! */
```

**Rule:** An `else` always pairs with the **nearest preceding unmatched `if`**, regardless of indentation. If you intend the `else` to belong to the outer `if`, you must use explicit braces:

```c
if (a > b)
{
    if (a > c)
        printf("a is the largest\n");
}
else
    printf("b might be the largest -- check further\n");
```

## 3.6 A Word of Caution

- Don't confuse `=` (assignment) with `==` (equality test).
- Watch for the classic **empty-statement bug**: `if (x > 0);  printf("positive\n");` — the semicolon right after `if (x > 0)` creates an *empty statement* as the "then" branch, so `printf` always executes unconditionally!
- Always use braces `{ }` for `if`/`else` bodies, even with a single statement, as a defensive coding habit — it prevents the "dangling statement" bug entirely.

## 3.7 Worked Programs

### Program 1: Largest of Three Numbers

```c
#include <stdio.h>
int main()
{
    int a, b, c;
    printf("Enter three numbers: ");
    scanf("%d %d %d", &a, &b, &c);

    if (a >= b)
    {
        if (a >= c)
            printf("%d is the largest.\n", a);
        else
            printf("%d is the largest.\n", c);
    }
    else
    {
        if (b >= c)
            printf("%d is the largest.\n", b);
        else
            printf("%d is the largest.\n", c);
    }
    return 0;
}
```

**Sample run:**
```
Enter three numbers: 12 45 30
45 is the largest.
```

### Program 2: Even or Odd

```c
#include <stdio.h>
int main()
{
    int n;
    printf("Enter an integer: ");
    scanf("%d", &n);

    if (n % 2 == 0)
        printf("%d is even.\n", n);
    else
        printf("%d is odd.\n", n);

    return 0;
}
```

### Program 3: Leap Year Check

```c
#include <stdio.h>
int main()
{
    int year;
    printf("Enter a year: ");
    scanf("%d", &year);

    if (year % 4 == 0)
    {
        if (year % 100 == 0)
        {
            if (year % 400 == 0)
                printf("%d is a leap year.\n", year);
            else
                printf("%d is NOT a leap year.\n", year);
        }
        else
        {
            printf("%d is a leap year.\n", year);
        }
    }
    else
    {
        printf("%d is NOT a leap year.\n", year);
    }
    return 0;
}
```

**Sample run:**
```
Enter a year: 2000
2000 is a leap year.
```

## 3.8 Key Takeaways

1. `if` executes a statement/block conditionally; `if-else` chooses between two alternatives.
2. Multiple statements under `if`/`else` **must** be enclosed in `{ }`.
3. `=` is assignment; `==` is comparison — mixing them up is a classic, hard-to-spot bug.
4. `if-else` can be nested arbitrarily deep to express multi-level decisions.
5. An `else` always binds to the nearest unmatched preceding `if`; use braces to remove ambiguity.
6. A stray semicolon right after `if (condition)` silently creates an empty "then" branch — a subtle but serious bug.
