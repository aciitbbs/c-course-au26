# Chapter 19 — Quiz: File Input/Output

## 📖 Topics Covered: `fopen` modes, `fgetc`/`fputc`, `fgets`/`fputs`, `fprintf`/`fscanf`, `fread`/`fwrite`, `fseek`

---

## Part A: Multiple Choice Questions (5)

### Q1. What does `fopen("data.txt", "w")` do if `data.txt` already exists and contains data?

A) It appends new data to the end, preserving existing content
B) It opens the file for reading only
C) It truncates (erases) the existing content, and subsequent writes start from an empty file
D) It raises a runtime error, refusing to open an existing file

<details>
<summary><b>Answer</b></summary>

**C) It truncates (erases) the existing content, and subsequent writes start from an empty file**

`"w"` mode always starts with an empty file — if the file already exists, its previous content is discarded (truncated) the moment it is successfully opened in this mode. Use `"a"` (append) instead to preserve existing content.
</details>

---

### Q2. Why must the return value of `fopen()` always be checked before using the file pointer?

A) `fopen` never fails, so this is unnecessary
B) `fopen` returns `NULL` if it fails to open the file, and using a `NULL` file pointer causes undefined behaviour
C) `fopen` returns `0` on success and `1` on failure
D) Checking is only needed when opening in binary mode

<details>
<summary><b>Answer</b></summary>

**B) `fopen` returns `NULL` if it fails to open the file, and using a `NULL` file pointer causes undefined behaviour**

Opening can fail for many reasons (file doesn't exist in read mode, insufficient permissions, disk issues, etc.). Attempting to read/write through a `NULL` pointer typically crashes the program, so checking `if (fp == NULL)` immediately after `fopen` is essential defensive practice.
</details>

---

### Q3. What is the correct function pairing for writing an entire `struct Student` variable directly as raw bytes, and reading it back later?

A) `fprintf` to write, `fscanf` to read
B) `fputs` to write, `fgets` to read
C) `fwrite` to write, `fread` to read
D) `putc` to write, `getc` to read

<details>
<summary><b>Answer</b></summary>

**C) `fwrite` to write, `fread` to read**

`fwrite`/`fread` operate on raw binary data of a specified size and count, making them the correct tools for directly persisting an entire structure's in-memory byte representation, rather than converting it to/from formatted text.
</details>

---

### Q4. What does `fseek(fp, 2 * sizeof(struct Student), SEEK_SET);` accomplish?

A) It reads the 2nd record from the file
B) It moves the file position indicator to the byte offset corresponding to the start of the 3rd record (index 2), counted from the beginning of the file
C) It deletes the first 2 records
D) It closes the file after 2 operations

<details>
<summary><b>Answer</b></summary>

**B) It moves the file position indicator to the byte offset corresponding to the start of the 3rd record (index 2), counted from the beginning of the file**

`SEEK_SET` means the offset is measured from the beginning of the file. Since each record occupies `sizeof(struct Student)` bytes, multiplying by `2` (the zero-based index) lands exactly at the start of the third record — but `fseek` itself only repositions the pointer; it does not read or delete anything by itself.
</details>

---

### Q5. Which mode should be used to add new records to the end of an existing log file without erasing what's already there?

A) `"w"`
B) `"r"`
C) `"a"`
D) `"r+"` only

<details>
<summary><b>Answer</b></summary>

**C) `"a"`**

`"a"` (append) mode positions all writes at the end of the file, preserving existing content, and creates the file if it doesn't already exist — exactly the behaviour needed for a growing log file.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Describe the three essential steps every C program must follow when working with a file, and explain the consequence of skipping the last step.

<details>
<summary><b>Model Answer</b></summary>

1. **Open** the file with `fopen()`, obtaining a `FILE *` handle (and checking it is not `NULL`).
2. **Read from or write to** the file using that handle, via functions like `fgetc`/`fputc`, `fgets`/`fputs`, `fprintf`/`fscanf`, or `fread`/`fwrite`.
3. **Close** the file with `fclose()` once finished.

**Consequence of skipping `fclose()`:** Many I/O libraries **buffer** data in memory before actually writing it to disk, for efficiency. If a program never calls `fclose()` (or otherwise never flushes its buffers), some or all of the data written during the session may **never actually reach the disk**, resulting in silent data loss — the file may appear empty or truncated even though the program seemingly "wrote" to it. Additionally, each open file consumes a limited OS resource (a file descriptor/handle); failing to close files can eventually exhaust this limit in long-running programs.
</details>

---

### Q2. Compare the file modes `"r"`, `"w"`, and `"a"`. What happens in each case if the target file does not already exist, and if it does?

<details>
<summary><b>Model Answer</b></summary>

| Mode | If file does NOT exist | If file DOES exist |
|---|---|---|
| `"r"` | `fopen` fails, returns `NULL` | Opens for reading; file position starts at the beginning |
| `"w"` | Creates a new, empty file | **Truncates** (erases) all existing content, then opens an effectively empty file for writing |
| `"a"` | Creates a new, empty file | Opens for writing; all writes are appended at the end, existing content is preserved |

The key distinction to remember is that `"w"` is destructive to existing content, while `"a"` is additive/preserving, and `"r"` requires the file to already exist.
</details>

---

### Q3. Explain the difference between text-mode and binary-mode file I/O in C, and give one scenario where binary mode is clearly the better choice.

<details>
<summary><b>Model Answer</b></summary>

**Text mode** (the default, e.g. `"r"`, `"w"`) treats file content as human-readable characters, and on some platforms (notably Windows) may perform newline translation between the in-memory `'\n'` representation and the platform's actual line-ending convention (`\r\n`) when reading/writing.

**Binary mode** (`"rb"`, `"wb"`, etc.) performs **no** such translation — every byte is read/written exactly as it exists in memory, with no character-level reinterpretation.

**Scenario favouring binary mode:** Saving an array of `struct Student` records directly via `fwrite`. Since a structure's raw byte layout (including any padding bytes, as discussed in Chapter 17) must be preserved *exactly* for `fread` to correctly reconstruct it later, any newline-translation "helpfulness" from text mode could corrupt the data if a byte inside the structure happened to match a newline character's binary value. Binary mode avoids this risk entirely, making it the correct choice for structured/raw data.
</details>

---

### Q4. Write a short explanation (with code) of how you would append a new line of text to an existing log file, without disturbing its previous contents.

<details>
<summary><b>Model Answer</b></summary>

```c
#include <stdio.h>

int main()
{
    FILE *fp = fopen("app.log", "a");   /* "a" mode: preserves existing content, writes go to the end */

    if (fp == NULL)
    {
        printf("Error: could not open log file.\n");
        return 1;
    }

    fprintf(fp, "User logged in at session start.\n");

    fclose(fp);
    return 0;
}
```

**Explanation:** Opening the file in `"a"` (append) mode ensures that any content already present in `app.log` from previous runs remains untouched — the internal file-position indicator for writes is always forced to the end of the file in this mode, regardless of where it might otherwise have been. The new line, written with `fprintf`, is therefore added *after* all existing content, rather than overwriting it (as `"w"` mode would do).
</details>

---

### Q5. A student's program crashes with a segmentation fault immediately after this code:

```c
FILE *fp = fopen("missing.txt", "r");
char ch = fgetc(fp);
```

Explain precisely why this crashes, and rewrite the code defensively to avoid the crash.

<details>
<summary><b>Model Answer</b></summary>

**Why it crashes:** `"missing.txt"` presumably does not exist (or cannot otherwise be opened for reading), so `fopen()` returns `NULL` instead of a valid `FILE *`. The next line, `fgetc(fp)`, then attempts to read through this `NULL` pointer — since `fgetc` needs to dereference internal file-stream data that a valid `FILE *` would point to, doing so with `NULL` results in undefined behaviour, which commonly manifests as a segmentation fault (crash).

**Defensive fix:**
```c
FILE *fp = fopen("missing.txt", "r");

if (fp == NULL)
{
    printf("Error: could not open 'missing.txt'. Please check the file exists.\n");
    return 1;    /* or otherwise handle the error without touching fp further */
}

char ch = fgetc(fp);   /* now safe, since fp is guaranteed non-NULL at this point */
fclose(fp);
```
Checking `fopen`'s return value immediately, before any further use of `fp`, is the standard, essential defensive pattern for all file-handling code in C.
</details>
