# Chapter 18 — Quiz: Console Input/Output

## 📖 Topics Covered: Formatted vs unformatted I/O, field width/precision, `sprintf`/`sscanf`, `getchar`/`putchar`, dangers of `gets`

---

## Part A: Multiple Choice Questions (5)

### Q1. What is the output of `printf("%5d\n", 42);`?

A) `42`
B) `   42` (three leading spaces, right-aligned in a field of width 5)
C) `42000`
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**B) `   42` (three leading spaces, right-aligned in a field of width 5)**

`%5d` specifies a minimum field width of 5 characters; since `42` is only 2 digits, it is right-aligned (padded with 3 leading spaces by default) to fill the field.
</details>

---

### Q2. Why is `gets()` considered dangerous and removed from the C11 standard library?

A) It only works with integers
B) It has no way to limit how many characters it reads, risking a buffer overflow if input exceeds the destination's size
C) It cannot read strings with spaces
D) It requires a special header not included in `<stdio.h>`

<details>
<summary><b>Answer</b></summary>

**B) It has no way to limit how many characters it reads, risking a buffer overflow if input exceeds the destination's size**

Unlike `fgets()`, which takes an explicit maximum size, `gets()` keeps reading and writing characters until a newline is found, with no bound on the destination buffer's capacity — a classic and serious security vulnerability.
</details>

---

### Q3. What do `sprintf` and `sscanf` do differently compared to `printf` and `scanf`?

A) They only work with integers
B) They read from/write to a string buffer instead of the console
C) They cannot use format specifiers
D) They automatically add a newline at the end

<details>
<summary><b>Answer</b></summary>

**B) They read from/write to a string buffer instead of the console**

`sprintf` formats data into a provided `char` buffer instead of printing to standard output; `sscanf` parses data out of a given string instead of reading from standard input. Otherwise, their format-specifier syntax is identical to `printf`/`scanf`.
</details>

---

### Q4. In the sequence `scanf("%d", &n); scanf("%c", &ch);`, why might `ch` unexpectedly receive a newline character instead of the next typed character?

A) `scanf("%c", ...)` always fails after a `%d`
B) The `%d` conversion leaves the trailing newline (from pressing Enter) still waiting in the input buffer, and the plain `%c` (without a leading space) reads that leftover newline directly
C) `%c` cannot be used after `%d` in the same program
D) `scanf` automatically clears the input buffer between calls

<details>
<summary><b>Answer</b></summary>

**B) The `%d` conversion leaves the trailing newline (from pressing Enter) still waiting in the input buffer, and the plain `%c` (without a leading space) reads that leftover newline directly**

`%d` stops consuming characters right after the number itself, leaving the newline character (typed when the user pressed Enter) still sitting in the input stream. A plain `%c` (unlike `%d`/`%f`) does **not** automatically skip leading whitespace, so it immediately reads that leftover `'\n'`. The fix is to use `" %c"` (with a leading space) to explicitly skip any pending whitespace first.
</details>

---

### Q5. What does `scanf`'s return value represent?

A) The total number of characters typed by the user
B) The number of format specifiers in the format string
C) The number of items successfully matched and assigned to variables
D) Always `0` on success, non-zero on failure

<details>
<summary><b>Answer</b></summary>

**C) The number of items successfully matched and assigned to variables**

Checking this return value against the expected number of conversions is a good defensive-programming practice, allowing a program to detect and handle malformed or unexpected input gracefully.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain the difference between "formatted" and "unformatted" console I/O in C, with one example function of each kind.

<details>
<summary><b>Model Answer</b></summary>

**Formatted I/O** functions (e.g., `printf`, `scanf`) use a **format string** containing conversion specifiers (`%d`, `%f`, `%s`, etc.) to control exactly how typed data is converted to/from its textual representation — they understand and convert between C's data types and text.

**Unformatted I/O** functions (e.g., `getchar`, `putchar`, `fgets`, `puts`) work with **raw characters or entire lines of text**, with no type conversion involved — they simply move characters in or out, byte by byte or line by line, without any notion of "this is an integer" or "this is a float."
</details>

---

### Q2. Why is `fgets()` strongly preferred over `gets()` for reading a line of text? Illustrate the risk `gets()` poses with a short example.

<details>
<summary><b>Model Answer</b></summary>

`gets(buffer)` has no parameter to specify how large `buffer` is, and it keeps copying characters from input into `buffer` until it encounters a newline — with **absolutely no check** on whether `buffer` still has room. If the user (or an attacker) types more characters than `buffer` can hold, `gets()` will happily keep writing past the buffer's end, corrupting adjacent memory — a classic **buffer overflow** vulnerability that has historically been exploited to compromise real software.

```c
char buffer[10];
gets(buffer);   /* if the user types more than 9 characters, memory beyond 'buffer' is silently overwritten */
```

`fgets(buffer, sizeof(buffer), stdin)` instead takes an explicit maximum size and **guarantees** it will never write more than that many characters (including the terminator) into `buffer`, making it a safe, bounded replacement — which is exactly why `gets()` was removed from the C11 standard entirely.
</details>

---

### Q3. Describe what `sprintf(buffer, "Score: %d/%d", obtained, total);` does, and give one practical reason a programmer might prefer this over directly calling `printf` with the same format string.

<details>
<summary><b>Model Answer</b></summary>

`sprintf` formats its arguments (`obtained`, `total`) according to the given format string, exactly as `printf` would — but instead of sending the resulting text to the console, it writes that formatted text into the `char` array `buffer`, terminating it with `'\0'`, so the result can be used as an ordinary string later.

**Practical reason to prefer `sprintf` over `printf`:** When the formatted text needs to be **reused** rather than immediately displayed — for example, building a filename, constructing a log message to write to a file later, assembling a string to pass to another function, or combining several pieces of formatted text before a single final `printf`/`fputs` call — `sprintf` lets you capture that formatted result as data, rather than sending it straight to the screen.
</details>

---

### Q4. Explain the meaning of the field-width and precision specifiers in `printf("%8.2f\n", value);`, using `value = 3.14159` as an example, and show the exact output.

<details>
<summary><b>Model Answer</b></summary>

In `%8.2f`:
- `8` is the **minimum field width** — the total output for this value will be padded with leading spaces (by default, right-aligned) to be at least 8 characters wide.
- `.2` is the **precision** for a floating-point conversion — exactly 2 digits will be shown after the decimal point (rounded as needed).

For `value = 3.14159`, the value is first rounded to 2 decimal places, giving `"3.14"` (4 characters). Since the field width requires at least 8 characters total, 4 leading spaces are added to pad it out:

**Output:** `    3.14` (4 leading spaces followed by `3.14`, for a total width of exactly 8 characters).
</details>

---

### Q5. A student writes `scanf("%d", &n); scanf("%c", &grade);` intending to read an integer followed by a single grade letter, but finds that `grade` always ends up holding a newline character instead of the letter the user typed. Explain why this happens and how to fix it.

<details>
<summary><b>Model Answer</b></summary>

**Cause:** After typing the integer and pressing Enter, the input stream still contains the newline character `'\n'` generated by that Enter key press — `%d` only consumes the digits themselves, leaving the newline unread and waiting in the buffer. The very next `scanf("%c", &grade)` call, using a **plain `%c`** (which, unlike `%d`/`%f`, does **not** automatically skip leading whitespace), immediately reads that leftover `'\n'` rather than waiting for the user's actual next keystroke.

**Fix:** Add a **leading space** before `%c` in the format string — `scanf(" %c", &grade);`. In `scanf` format strings, whitespace (including a single space) matches *any amount* of whitespace (including none), so it explicitly consumes and discards the leftover newline (and any other stray whitespace) before reading the actual next non-whitespace character into `grade`.
</details>
