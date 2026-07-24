# Chapter 4 — Lecture: More Complex Decision Making

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 4: "More Complex Decision Making"

---

## 4.1 Use of Logical Operators — Checking Ranges

Real decisions often depend on **multiple conditions** combined together. C provides three **logical operators** for this:

| Operator | Meaning | Example |
|---|---|---|
| `&&` | Logical AND — true only if **both** operands are true | `(age >= 18 && age <= 60)` |
| `\|\|` | Logical OR — true if **at least one** operand is true | `(marks < 0 \|\| marks > 100)` |
| `!` | Logical NOT — reverses true/false | `!(isValid)` |

```c
#include <stdio.h>
int main()
{
    int age;
    printf("Enter age: ");
    scanf("%d", &age);

    if (age >= 18 && age <= 60)
        printf("Eligible to work.\n");
    else
        printf("Not eligible to work.\n");

    return 0;
}
```

### Truth Tables

| A | B | `A && B` | `A \|\| B` |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 |

| A | `!A` |
|---|---|
| 0 | 1 |
| non-zero | 0 |

### Short-Circuit Evaluation

C guarantees **short-circuit evaluation** for `&&` and `||`:

- In `A && B`: if `A` is false, `B` is **never evaluated** (the whole expression is already false).
- In `A || B`: if `A` is true, `B` is **never evaluated** (the whole expression is already true).

```c
int x = 0;
if (x != 0 && 10 / x > 1)   /* 10/x is NEVER evaluated because x != 0 is false first -- avoids divide-by-zero! */
    printf("Safe check passed\n");
```

This is not just an optimization — it is a language guarantee you can rely on to write safe guard conditions.

## 4.2 The `else if` Clause

Chaining several `if-else` blocks with `else if` lets you test a **sequence of conditions**, stopping at the first one that is true:

```c
if (condition1)
    statement1;
else if (condition2)
    statement2;
else if (condition3)
    statement3;
else
    default-statement;
```

```c
#include <stdio.h>
int main()
{
    int marks;
    printf("Enter marks: ");
    scanf("%d", &marks);

    if (marks >= 90)
        printf("Grade A\n");
    else if (marks >= 75)
        printf("Grade B\n");
    else if (marks >= 60)
        printf("Grade C\n");
    else if (marks >= 40)
        printf("Grade D\n");
    else
        printf("Grade F\n");

    return 0;
}
```

> The `else if` ladder is really just nested `if-else` written on successive lines for readability — the compiler sees it as `else { if (...) ... }`.

## 4.3 Use of Logical Operators — Yes/No Problems

Logical operators are especially useful for validating "Yes/No" or menu-style character input, where either uppercase or lowercase should be accepted:

```c
#include <stdio.h>
int main()
{
    char choice;
    printf("Do you want to continue (Y/N)? ");
    scanf(" %c", &choice);

    if (choice == 'y' || choice == 'Y')
        printf("Continuing...\n");
    else if (choice == 'n' || choice == 'N')
        printf("Exiting...\n");
    else
        printf("Invalid choice.\n");

    return 0;
}
```

## 4.4 The `!` Operator

`!` (logical NOT) inverts a truth value: it converts a **zero** value to `1`, and any **non-zero** value to `0`.

```c
int flag = 0;
if (!flag)
    printf("flag is false, so !flag is true\n");
```

`!` is frequently used to make guard conditions read naturally:

```c
if (!(n >= 1 && n <= 100))
    printf("n is out of the valid range 1-100\n");
```

By De Morgan's laws, `!(A && B)` is equivalent to `(!A || !B)`, and `!(A || B)` is equivalent to `(!A && !B)` — useful for simplifying negated compound conditions.

## 4.5 Hierarchy of Operators Revisited

With logical and relational operators added, the precedence table (high to low) now looks like:

| Precedence (high→low) | Operators |
|---|---|
| 1 | `()` |
| 2 | `!`, unary `+`/`-`, `++`, `--` |
| 3 | `*`, `/`, `%` |
| 4 | `+`, `-` |
| 5 | `<`, `<=`, `>`, `>=` |
| 6 | `==`, `!=` |
| 7 | `&&` |
| 8 | `\|\|` |
| 9 | `?:` (ternary) |
| 10 | `=`, `+=`, `-=`, ... |

```c
if (marks >= 40 && marks <= 100 || attendance >= 75)
    /* relational (>=, <=) evaluated first, then && , then || , per precedence */
```

Always add parentheses to compound conditions for clarity, even when precedence would technically give the correct grouping — it drastically reduces bugs during exams and real projects alike.

## 4.6 The Conditional (Ternary) Operator `?:`

The **only ternary (3-operand) operator** in C provides a compact way to write simple `if-else` expressions:

```c
expression = (condition) ? true-value : false-value;
```

```c
int a = 15, b = 20, larger;
larger = (a > b) ? a : b;      /* larger = 20 */

int n = 7;
char* label = (n % 2 == 0) ? "Even" : "Odd";
printf("%s\n", label);
```

The ternary operator is an **expression**, not a statement — it produces a value that can be assigned, printed, or used inside another expression. It is best used for short, simple choices; for anything more complex, prefer a regular `if-else` for readability.

## 4.7 Worked Programs

### Program 1: Validating a Range with Logical Operators

```c
#include <stdio.h>
int main()
{
    int marks;
    printf("Enter marks (0-100): ");
    scanf("%d", &marks);

    if (marks < 0 || marks > 100)
        printf("Invalid input.\n");
    else if (marks >= 40)
        printf("Result: PASS\n");
    else
        printf("Result: FAIL\n");

    return 0;
}
```

### Program 2: Vowel or Consonant (using `||` and ternary)

```c
#include <stdio.h>
int main()
{
    char ch;
    printf("Enter an alphabet: ");
    scanf(" %c", &ch);

    if (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u' ||
        ch == 'A' || ch == 'E' || ch == 'I' || ch == 'O' || ch == 'U')
        printf("%c is a Vowel.\n", ch);
    else
        printf("%c is a Consonant.\n", ch);

    return 0;
}
```

### Program 3: Largest of Three Using the Ternary Operator

```c
#include <stdio.h>
int main()
{
    int a, b, c, largest;
    printf("Enter three numbers: ");
    scanf("%d %d %d", &a, &b, &c);

    largest = (a > b) ? ((a > c) ? a : c) : ((b > c) ? b : c);

    printf("Largest = %d\n", largest);
    return 0;
}
```

## 4.8 Key Takeaways

1. `&&`, `||`, and `!` combine multiple conditions; C guarantees short-circuit evaluation for `&&` and `||`.
2. Short-circuiting can be used deliberately to guard against unsafe operations (e.g., division by zero).
3. `else if` ladders test a sequence of mutually exclusive conditions, stopping at the first true one.
4. The ternary operator `?:` is a compact expression-level substitute for simple `if-else` — use it sparingly for clarity.
5. Always parenthesize compound logical expressions, even when not strictly required, for readability and to avoid precedence mistakes.
