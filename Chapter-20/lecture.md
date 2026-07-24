# Chapter 20 — Lecture: More Issues In Input/Output

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 20: "More Issues In Input/Output"

---

## 20.1 Using `argc` and `argv` — Command-Line Arguments

Every C program can accept **command-line arguments** — text provided when the program is launched, before it even starts running. This is captured through a special, extended form of `main()`:

```c
int main(int argc, char *argv[])
{
    /* ... */
    return 0;
}
```

- `argc` ("argument count") — the number of command-line tokens, **including the program's own name**.
- `argv` ("argument vector") — an **array of strings** (`char *`), where `argv[0]` is the program's name/path, and `argv[1], argv[2], ...` are the actual arguments supplied.

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    int i;
    printf("Number of arguments: %d\n", argc);
    for (i = 0; i < argc; i++)
        printf("argv[%d] = %s\n", i, argv[i]);
    return 0;
}
```

**Running:** `./program hello 123 world`

**Output:**
```
Number of arguments: 4
argv[0] = ./program
argv[1] = hello
argv[2] = 123
argv[3] = world
```

> **Important:** every element of `argv` is a **string**, even numeric-looking arguments like `"123"`. To use such an argument as a number, you must explicitly convert it, e.g., with `atoi()` or `atof()` from `<stdlib.h>`.

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    if (argc != 2)
    {
        printf("Usage: %s <number>\n", argv[0]);
        return 1;
    }

    int n = atoi(argv[1]);   /* convert the string argument to an int */
    printf("Square of %d = %d\n", n, n * n);
    return 0;
}
```

## 20.2 Detecting Errors in Reading/Writing

Beyond checking `fopen`'s return value, C provides tools to detect errors and end-of-file conditions **during** reading/writing:

| Function | Purpose |
|---|---|
| `feof(fp)` | Returns non-zero if the end of the file has been reached |
| `ferror(fp)` | Returns non-zero if an error occurred on the stream |
| `clearerr(fp)` | Clears the error/EOF indicators for the stream |

```c
if (feof(fp))
    printf("Reached end of file.\n");
if (ferror(fp))
    printf("An error occurred while reading/writing.\n");
```

> **Best practice:** rely on the **return value** of the actual read function (e.g., `fscanf() != EOF`, `fread() == expectedCount`, `fgets() != NULL`) as the primary loop-termination check, rather than calling `feof()` *before* attempting a read — checking `feof()` too early is a very common, subtle bug that can cause the last valid item to be processed twice or a spurious "extra" iteration.

## 20.3 Standard File Pointers

Three file streams are automatically opened for every C program, without any explicit `fopen()`:

| Stream | Purpose | Default Destination |
|---|---|---|
| `stdin` | Standard input | Keyboard |
| `stdout` | Standard output | Screen/console |
| `stderr` | Standard error | Screen/console (kept separate from `stdout` for redirection purposes) |

```c
fprintf(stdout, "Normal message.\n");   /* equivalent to printf("Normal message.\n"); */
fprintf(stderr, "Error: something went wrong!\n");   /* goes to the error stream */
```

## 20.4 I/O Redirection

Operating systems (Windows Command Prompt, Linux/macOS shells) let you **redirect** these standard streams using special symbols on the command line — without changing the program's code at all.

### Redirecting Output

```bash
./program > output.txt      # stdout is redirected to a file, instead of the screen
```

### Redirecting Input

```bash
./program < input.txt       # stdin is read from a file, instead of the keyboard
```

### Both Ways at Once

```bash
./program < input.txt > output.txt
```

This is extremely useful for **automated testing** — you can prepare a fixed input file and compare the program's output file against an expected result, without manually typing input every time.

## 20.5 Worked Programs

### Program 1: Command-Line Calculator

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    if (argc != 4)
    {
        printf("Usage: %s <num1> <operator> <num2>\n", argv[0]);
        return 1;
    }

    float a = atof(argv[1]);
    char op = argv[2][0];
    float b = atof(argv[3]);
    float result;

    switch (op)
    {
        case '+': result = a + b; break;
        case '-': result = a - b; break;
        case '*': result = a * b; break;
        case '/':
            if (b == 0) { printf("Error: division by zero.\n"); return 1; }
            result = a / b;
            break;
        default:
            printf("Invalid operator.\n");
            return 1;
    }

    printf("Result = %.2f\n", result);
    return 0;
}
```

**Running:** `./calc 10 + 5` → `Result = 15.00`

### Program 2: Robust File Reading with Error Checking

```c
#include <stdio.h>

int main()
{
    FILE *fp = fopen("data.txt", "r");
    int value, sum = 0, count = 0;

    if (fp == NULL)
    {
        fprintf(stderr, "Error: could not open data.txt\n");
        return 1;
    }

    while (fscanf(fp, "%d", &value) == 1)   /* check the RETURN VALUE, not feof() beforehand */
    {
        sum += value;
        count++;
    }

    if (ferror(fp))
        fprintf(stderr, "Warning: a read error occurred.\n");

    fclose(fp);

    if (count > 0)
        printf("Sum = %d, Average = %.2f\n", sum, (float) sum / count);
    else
        printf("No valid integers found in file.\n");

    return 0;
}
```

## 20.6 Key Takeaways

1. `int main(int argc, char *argv[])` lets a program receive command-line arguments; `argc` counts them (including the program name), `argv` holds them all as strings.
2. Command-line arguments are always strings — convert them with `atoi`/`atof` before using them as numbers.
3. `feof()`/`ferror()` help diagnose end-of-file and error conditions, but should generally supplement (not replace) checking the read function's own return value as the loop-termination test.
4. `stdin`, `stdout`, and `stderr` are automatically available standard streams; `stderr` is kept separate so error messages remain visible even when `stdout` is redirected.
5. Shell redirection (`<`, `>`) lets a program's standard input/output be connected to files without modifying the program's source code — invaluable for automated testing.
