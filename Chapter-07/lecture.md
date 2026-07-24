# Chapter 7 — Lecture: Case Control Instruction

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 7: "Case Control Instruction"

---

## 7.1 Decisions Using `switch`

When a program must choose among **several discrete, known values** of a single variable/expression, a long `if-else-if` ladder can be replaced with a cleaner **`switch` statement**:

```c
switch (expression)
{
    case constant1:
        statement(s);
        break;
    case constant2:
        statement(s);
        break;
    ...
    default:
        statement(s);
}
```

- `expression` must evaluate to an **integer type** (`int` or `char`; not `float`/`double`, and not strings).
- Each `case` label must be a **constant** (a literal value or a `const`/`#define`d constant expression), and all case values within one `switch` must be **distinct**.
- `default` is optional and executes when no `case` matches; it may appear anywhere but conventionally goes last.

```c
#include <stdio.h>
int main()
{
    int day;
    printf("Enter day number (1-7): ");
    scanf("%d", &day);

    switch (day)
    {
        case 1: printf("Monday\n"); break;
        case 2: printf("Tuesday\n"); break;
        case 3: printf("Wednesday\n"); break;
        case 4: printf("Thursday\n"); break;
        case 5: printf("Friday\n"); break;
        case 6: printf("Saturday\n"); break;
        case 7: printf("Sunday\n"); break;
        default: printf("Invalid day number.\n");
    }
    return 0;
}
```

### 7.1.1 The Tips and Traps

**Trap 1 — Forgetting `break` causes "fall-through":** Execution continues into the *next* `case`'s statements even if its label doesn't match, until a `break` (or the end of the `switch`) is reached.

```c
switch (grade)
{
    case 'A':
    case 'B':
        printf("Well done!\n");    /* runs for BOTH 'A' and 'B' -- intentional fall-through */
        break;
    case 'C':
        printf("You passed.\n");
        break;
    default:
        printf("Better try again.\n");
}
```

Here, stacking `case 'A':` directly above `case 'B':` **without a break in between** is a deliberate, idiomatic use of fall-through to group multiple cases together.

**Trap 2 — Accidental fall-through (a real bug):**
```c
switch (choice)
{
    case 1:
        printf("Option 1\n");
        /* forgot break! */
    case 2:
        printf("Option 2\n");
        break;
}
```
If `choice == 1`, this prints **both** `"Option 1"` and `"Option 2"`, because execution falls through into `case 2`'s code after finishing `case 1`'s.

**Trap 3 — `switch` only tests equality, not ranges:** `case 1 ... 10:` is **not** standard C (it is a GCC extension, not portable). Use `if-else` for range checks.

## 7.2 `switch` versus `if-else` Ladder

| Aspect | `switch` | `if-else` ladder |
|---|---|---|
| Tests | Equality against constants only | Any condition, including ranges, `&&`/`\|\|` |
| Data types | `int`/`char` (integral) only | Any type, any expression |
| Readability | Cleaner for many discrete options | Better for ranges/complex logic |
| Fall-through | Possible (and sometimes useful) | Not applicable |

Rule of thumb: use `switch` when testing one variable against many specific constant values (e.g., menu choices, day numbers); use `if-else` for ranges or multi-variable logic.

## 7.3 The `goto` Statement

`goto` transfers control **unconditionally** to a labelled statement anywhere within the same function.

```c
goto label;
...
label:
    statement;
```

```c
#include <stdio.h>
int main()
{
    int i = 1;

    start:
    if (i <= 5)
    {
        printf("%d ", i);
        i++;
        goto start;
    }
    printf("\n");
    return 0;
}
```

**Output:** `1 2 3 4 5`

> **Strong caution:** `goto` can make programs hard to read, debug, and maintain ("spaghetti code") because it breaks the normal top-to-bottom / structured flow. Modern C style **strongly discourages** `goto` for loops (use `while`/`for`/`do-while` instead) — it is included here for completeness and historical/legacy-code understanding, and its one broadly accepted legitimate use is jumping to a single centralized cleanup/error-handling label near the end of a function.

```c
/* One accepted, idiomatic use of goto: centralized error handling */
#include <stdio.h>
int main()
{
    FILE *fp = NULL;   /* FILE and fopen are formally covered in Chapter 19 */
    int errorOccurred = 0;

    /* ... some resource-acquisition steps that might set errorOccurred = 1 ... */

    if (errorOccurred)
        goto cleanup;

    printf("Processing succeeded.\n");

    cleanup:
    printf("Cleaning up resources...\n");
    return 0;
}
```

## 7.4 Worked Programs

### Program 1: Simple Calculator Using `switch`

```c
#include <stdio.h>
int main()
{
    float a, b, result;
    char op;

    printf("Enter expression (e.g. 4 + 5): ");
    scanf("%f %c %f", &a, &op, &b);

    switch (op)
    {
        case '+': result = a + b; break;
        case '-': result = a - b; break;
        case '*': result = a * b; break;
        case '/':
            if (b == 0)
            {
                printf("Error: division by zero.\n");
                return 0;
            }
            result = a / b;
            break;
        default:
            printf("Invalid operator.\n");
            return 0;
    }
    printf("Result = %.2f\n", result);
    return 0;
}
```

### Program 2: Vowel/Consonant Using `switch` with Fall-Through

```c
#include <stdio.h>
int main()
{
    char ch;
    printf("Enter an alphabet: ");
    scanf(" %c", &ch);

    switch (ch)
    {
        case 'a': case 'e': case 'i': case 'o': case 'u':
        case 'A': case 'E': case 'I': case 'O': case 'U':
            printf("%c is a Vowel.\n", ch);
            break;
        default:
            printf("%c is a Consonant.\n", ch);
    }
    return 0;
}
```

## 7.5 Key Takeaways

1. `switch` compares one integral expression against several constant `case` labels — ideal for menu/option-style logic.
2. Without `break`, execution "falls through" into the next case — sometimes intentional (grouping cases), sometimes a bug.
3. `switch` cannot test ranges or floating-point/string values directly; use `if-else` for those.
4. `goto` performs an unconditional jump to a label; it is legal but discouraged in modern structured C, except for a few accepted idioms like centralized cleanup.
5. Always include a `default` case in `switch` to gracefully handle unexpected input.
