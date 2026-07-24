# Chapter 20 — Quiz: More Issues In Input/Output

## 📖 Topics Covered: `argc`/`argv`, `feof`/`ferror`, standard streams (`stdin`/`stdout`/`stderr`), I/O redirection

---

## Part A: Multiple Choice Questions (5)

### Q1. For the command `./program alpha beta`, what is the value of `argc` inside `main(int argc, char *argv[])`?

A) `2`
B) `3`
C) `1`
D) `0`

<details>
<summary><b>Answer</b></summary>

**B) `3`**

`argc` counts all tokens including the program's own name (`argv[0] = "./program"`), plus `"alpha"` and `"beta"`, totaling 3.
</details>

---

### Q2. Why can't `argv[1]` be used directly in arithmetic like `int x = argv[1] + 5;`?

A) `argv[1]` does not exist unless `argc >= 5`
B) `argv[1]` is a string (`char *`), not a number — it must first be converted with a function like `atoi`
C) Command-line arguments cannot contain digits
D) `argv` is read-only and cannot be used in expressions at all

<details>
<summary><b>Answer</b></summary>

**B) `argv[1]` is a string (`char *`), not a number — it must first be converted with a function like `atoi`**

Every element of `argv` is a null-terminated string, regardless of what it "looks like." To use it as a numeric value, it must be explicitly parsed/converted, e.g., `int x = atoi(argv[1]) + 5;`.
</details>

---

### Q3. What is the recommended way to terminate a file-reading loop: checking `feof(fp)` before each read, or checking the read function's own return value?

A) Always check `feof(fp)` before each read — it is always safe and equivalent
B) Check the read function's own return value (e.g., `fscanf() != EOF`) as the primary condition — checking `feof()` beforehand can cause the last item to be processed twice or an extra spurious iteration
C) Neither method works reliably in C
D) `feof()` must always be called after `fclose()`

<details>
<summary><b>Answer</b></summary>

**B) Check the read function's own return value (e.g., `fscanf() != EOF`) as the primary condition — checking `feof()` beforehand can cause the last item to be processed twice or an extra spurious iteration**

`feof()` only becomes true *after* a read attempt has already failed due to reaching the end of the file — checking it *before* attempting the read can lead to an extra, incorrect iteration processing stale/leftover data. The read function's own return value directly and correctly signals success or failure for that specific attempt.
</details>

---

### Q4. Why is `stderr` kept as a separate stream from `stdout`, rather than combining error messages into the same stream?

A) `stderr` is faster than `stdout`
B) So that error messages remain visible on the console even if `stdout` has been redirected to a file
C) `stderr` cannot be used with `fprintf`
D) There is no real reason; it is purely historical and has no practical benefit

<details>
<summary><b>Answer</b></summary>

**B) So that error messages remain visible on the console even if `stdout` has been redirected to a file**

If a user runs `./program > output.txt`, only `stdout` is redirected into `output.txt`; `stderr` (used for error/diagnostic messages) still appears on the console, ensuring the user notices errors even while normal output is being captured to a file.
</details>

---

### Q5. What does the shell command `./program < input.txt > output.txt` do?

A) It runs `program` and displays both input and output on the console
B) It redirects `program`'s standard input to come from `input.txt`, and redirects its standard output into `output.txt`
C) It copies `input.txt` into `output.txt` without running `program`
D) It is invalid syntax; only one redirection is allowed per command

<details>
<summary><b>Answer</b></summary>

**B) It redirects `program`'s standard input to come from `input.txt`, and redirects its standard output into `output.txt`**

The `<` symbol redirects `stdin` to read from the named file instead of the keyboard, and `>` redirects `stdout` to write into the named file instead of the console — both can be combined in a single command, and the program itself needs no code changes to support this.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain the roles of `argc` and `argv` in `int main(int argc, char *argv[])`, and write a short program skeleton that prints an error and exits if the user does not supply exactly one command-line argument.

<details>
<summary><b>Model Answer</b></summary>

`argc` ("argument count") tells the program how many command-line tokens were supplied when it was launched, **including the program's own name** as the first token. `argv` ("argument vector") is an array of `char *` strings holding the actual text of each token — `argv[0]` is always the program's own invocation name/path, and `argv[1]` through `argv[argc-1]` are the actual arguments the user provided.

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    if (argc != 2)     /* expects exactly 1 real argument, plus the program name = 2 total */
    {
        printf("Usage: %s <argument>\n", argv[0]);
        return 1;
    }

    printf("You provided: %s\n", argv[1]);
    return 0;
}
```
</details>

---

### Q2. Why must command-line arguments be explicitly converted before being used numerically? Illustrate with a short, correct example using `atoi`.

<details>
<summary><b>Model Answer</b></summary>

Every element of `argv` is stored and passed as a **string** (`char *`), regardless of whether its text happens to represent a number, a word, or anything else — the operating system and the C runtime have no built-in concept of "this particular argument is meant to be numeric." Attempting to use `argv[i]` directly in arithmetic would either be a type error or would operate on the pointer's address value, not the numeric text it represents — neither of which is useful.

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[])
{
    if (argc != 2)
        return 1;

    int n = atoi(argv[1]);      /* explicitly parses the string "42" into the integer 42 */
    printf("Doubled: %d\n", n * 2);
    return 0;
}
```

Running `./program 42` correctly prints `Doubled: 84`, because `atoi` performed the necessary string-to-integer conversion before the multiplication.
</details>

---

### Q3. Describe the purpose of `feof()` and `ferror()`, and explain the classic mistake of using `feof()` as the primary loop condition for reading a file.

<details>
<summary><b>Model Answer</b></summary>

`feof(fp)` reports whether the end-of-file indicator has been set on the stream `fp` — but critically, this indicator is only set **after** a read operation has already *attempted* to read past the end of the file and failed; it is not "looking ahead" to predict the next read's outcome. `ferror(fp)` similarly reports whether an error indicator has been set due to some prior failed operation.

**Classic mistake:**
```c
while (!feof(fp))                 /* WRONG pattern */
{
    fscanf(fp, "%d", &value);
    sum += value;                  /* may process a STALE/garbage 'value' on the final, failed iteration */
}
```
Here, `feof(fp)` is still false *before* the final, failing `fscanf` call (since EOF hasn't been "hit" yet), so the loop body executes one extra time with `value` left unchanged (still holding its previous value) or uninitialized — silently processing incorrect data.

**Correct pattern:** Check the read function's own return value directly:
```c
while (fscanf(fp, "%d", &value) == 1)    /* CORRECT: only proceeds if a value was ACTUALLY read */
{
    sum += value;
}
```
</details>

---

### Q4. Explain what `stdin`, `stdout`, and `stderr` are, and why a well-written C program should send error/diagnostic messages to `stderr` rather than `stdout`.

<details>
<summary><b>Model Answer</b></summary>

`stdin`, `stdout`, and `stderr` are three **standard file streams** that every C program has automatically available, without needing to call `fopen()`: `stdin` is the default source of input (normally the keyboard), `stdout` is the default destination for normal program output (normally the console), and `stderr` is a *separate* stream specifically intended for error and diagnostic messages (also normally shown on the console by default).

**Why send errors to `stderr` instead of `stdout`:** Because `stdout` and `stderr` can be **independently redirected** by the user running the program (e.g., `./program > results.txt`), sending error messages to `stderr` ensures they remain visible on the console (or can be redirected/logged separately, e.g., `2> errors.txt`) even while the program's normal output is being captured into a file. If errors were mixed into `stdout`, they would silently end up inside `results.txt` along with the legitimate output, potentially going unnoticed by the user.
</details>

---

### Q5. Explain how I/O redirection (`<` and `>`) can be used to automate testing of a C program that reads several integers from `stdin` and prints their sum to `stdout`, without manually typing input each time.

<details>
<summary><b>Model Answer</b></summary>

Since the program simply reads from `stdin` and writes to `stdout` using ordinary functions like `scanf`/`printf`, it has **no way of knowing** (and does not need to know) whether those streams are connected to the keyboard/console or to files — this is entirely controlled by the shell that launches the program.

To automate testing:
1. Prepare a text file, e.g., `test_input.txt`, containing exactly the sequence of numbers the program would expect to be typed in, in the same order.
2. Run the program with input redirected from that file and output redirected into a results file: `./sumProgram < test_input.txt > actual_output.txt`.
3. Compare `actual_output.txt` against a previously prepared `expected_output.txt` (e.g., using a file-comparison tool) to automatically verify correctness, without a human needing to type input or visually inspect console output for every test run.

This technique — feeding a program pre-written input files and capturing its output to files — is the foundation of simple automated regression testing for command-line programs.
</details>
