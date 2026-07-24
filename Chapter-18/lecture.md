# Chapter 18 — Lecture: Console Input/Output

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 18: "Console Input/Output"

---

## 18.1 Types of I/O

C classifies console I/O functions into two categories:

| Category | Description | Examples |
|---|---|---|
| **Formatted I/O** | Uses a format string to control conversion between text and typed data | `printf`, `scanf`, `sprintf`, `sscanf` |
| **Unformatted I/O** | Reads/writes raw characters or strings with no type conversion/formatting | `getchar`, `putchar`, `gets`/`fgets`, `puts` |

## 18.2 Formatted Console I/O Functions

### `printf()` — A Closer Look at Format Specifiers

| Specifier | Type | Example |
|---|---|---|
| `%d`, `%i` | `int` | `printf("%d", 42);` |
| `%u` | `unsigned int` | `printf("%u", 4000000000u);` |
| `%f` | `float`/`double` | `printf("%f", 3.14);` |
| `%e` | scientific notation | `printf("%e", 12345.6789);` → `1.234568e+04` |
| `%c` | `char` | `printf("%c", 'A');` |
| `%s` | string | `printf("%s", "hi");` |
| `%x`, `%X` | hexadecimal | `printf("%x", 255);` → `ff` |
| `%o` | octal | `printf("%o", 8);` → `10` |
| `%p` | pointer address | `printf("%p", &x);` |
| `%%` | literal `%` | `printf("100%%");` → `100%` |

### Field Width and Precision

```c
printf("%5d\n", 42);      /* right-aligned in a field of width 5:   "   42" */
printf("%-5d|\n", 42);    /* left-aligned in a field of width 5:    "42   |" */
printf("%05d\n", 42);     /* zero-padded:                            "00042" */
printf("%.2f\n", 3.14159); /* precision: 2 digits after decimal:     "3.14" */
printf("%8.2f\n", 3.14159); /* width 8, precision 2:                "    3.14" */
```

### `scanf()` — A Closer Look

```c
int a; float b; char c;
scanf("%d %f %c", &a, &b, &c);   /* whitespace in the format string is flexible; matches any amount of whitespace in input */
```

- `scanf` **skips leading whitespace** automatically for numeric (`%d`, `%f`) and character-with-leading-space (`" %c"`) conversions, but **not** for a plain `%c` without a leading space (which reads the very next character, whitespace or not).
- `scanf` returns the **number of items successfully matched and assigned** — checking this return value is good practice for robust input validation.

```c
int x;
int itemsRead = scanf("%d", &x);
if (itemsRead != 1)
    printf("Invalid input!\n");
```

## 18.3 `sprintf()` and `sscanf()` — String Versions

`sprintf` and `sscanf` behave exactly like `printf`/`scanf`, but **read from/write to a string buffer** instead of the console:

```c
#include <stdio.h>
int main()
{
    char buffer[50];
    int age = 20;
    float cgpa = 8.5;

    sprintf(buffer, "Age: %d, CGPA: %.1f", age, cgpa);
    printf("%s\n", buffer);       /* prints: Age: 20, CGPA: 8.5 */

    int parsedAge;
    float parsedCgpa;
    sscanf(buffer, "Age: %d, CGPA: %f", &parsedAge, &parsedCgpa);
    printf("Parsed: %d, %.1f\n", parsedAge, parsedCgpa);

    return 0;
}
```

`sprintf` is extremely useful for **building formatted strings** (e.g., log messages, file names) that will be used elsewhere rather than printed immediately; `sscanf` is useful for **parsing** structured text.

## 18.4 Unformatted Console I/O Functions

| Function | Purpose |
|---|---|
| `getchar()` | Reads a single character from standard input |
| `putchar(ch)` | Writes a single character to standard output |
| `gets(str)` | Reads a line into `str` — **deprecated/unsafe**, no bounds checking; avoid it |
| `fgets(str, n, stdin)` | Reads up to `n-1` characters (or until newline) — the **safe** replacement for `gets` |
| `puts(str)` | Writes a string followed by a newline |

```c
#include <stdio.h>
int main()
{
    char ch;

    printf("Enter a character: ");
    ch = getchar();

    printf("You entered: ");
    putchar(ch);
    putchar('\n');

    return 0;
}
```

```c
#include <stdio.h>
int main()
{
    char line[100];

    printf("Enter a line: ");
    fgets(line, sizeof(line), stdin);

    printf("You entered: ");
    puts(line);   /* NOTE: fgets keeps the newline, so puts may produce an extra blank line */

    return 0;
}
```

> **Why `gets()` is dangerous and removed from the C11 standard library:** `gets()` has **no way to limit** how many characters it reads — if the input line is longer than the destination buffer, it keeps writing past the buffer's end, causing a severe buffer overflow. `fgets()` requires you to specify the buffer size, making it a safe, bounded replacement.

## 18.5 Worked Programs

### Program 1: Character Counting Using `getchar`/`putchar`

```c
#include <stdio.h>

int main()
{
    char ch;
    int count = 0;

    printf("Type characters (Ctrl+D / Ctrl+Z then Enter to stop):\n");

    while ((ch = getchar()) != EOF)
    {
        putchar(ch);
        if (ch != '\n')
            count++;
    }

    printf("\nTotal non-newline characters typed: %d\n", count);

    return 0;
}
```

### Program 2: Building a Formatted Report String with `sprintf`

```c
#include <stdio.h>

int main()
{
    char report[100];
    char name[30] = "Ayan";
    int marks = 87;

    sprintf(report, "Student: %-10s Marks: %3d/100", name, marks);
    puts(report);

    return 0;
}
```

**Output:** `Student: Ayan       Marks:  87/100`

## 18.6 Common Pitfalls

| Pitfall | Example | Fix |
|---|---|---|
| Using `gets()` | `gets(buffer);` | Use `fgets(buffer, sizeof(buffer), stdin);` instead |
| Plain `%c` in `scanf` after a numeric read leaves a leftover newline | `scanf("%d", &n); scanf("%c", &ch);` reads the leftover `'\n'` as `ch` | Use `scanf(" %c", &ch);` (leading space skips whitespace) |
| Ignoring `scanf`'s return value | Assuming input always succeeds | Check `if (scanf(...) != expectedCount)` for robust programs |
| Confusing `%e` (scientific notation) with `%f` | Expecting `%f`-style output from `%e` | `%e` always prints `d.ddddddE±dd` form |

## 18.7 Key Takeaways

1. Console I/O splits into **formatted** (`printf`/`scanf`, type-aware, format-string driven) and **unformatted** (`getchar`/`putchar`/`fgets`/`puts`, raw character/line-based) functions.
2. `sprintf`/`sscanf` redirect formatted I/O to/from a string buffer instead of the console — invaluable for building or parsing text.
3. `gets()` is dangerously unsafe (no bounds checking) and has been removed from modern C standards; always use `fgets()` instead.
4. `scanf`'s return value (the count of successfully matched items) should be checked for robust input validation.
5. A leading space in a `scanf` format string (`" %c"`) skips any pending whitespace, including leftover newlines from a previous numeric read.
