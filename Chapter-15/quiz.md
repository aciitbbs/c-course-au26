# Chapter 15 — Quiz: Strings

## 📖 Topics Covered: String representation, `'\0'` terminator, `scanf`/`fgets`, `<string.h>` functions, string comparison

---

## Part A: Multiple Choice Questions (5)

### Q1. How many bytes of storage are needed for the string `"Hello"` as a C string, including its terminator?

A) 5
B) 6
C) 10
D) 4

<details>
<summary><b>Answer</b></summary>

**B) 6**

"Hello" has 5 visible characters, plus the mandatory null terminator `'\0'`, totaling 6 bytes.
</details>

---

### Q2. What is wrong with using `if (str1 == str2)` to check whether two C strings have the same content?

A) Nothing — this is the correct and only way to compare strings
B) It compares the pointer addresses of `str1` and `str2`, not their actual character contents
C) `==` cannot be used with `char` arrays at all; it is a syntax error
D) It always returns `true` regardless of content

<details>
<summary><b>Answer</b></summary>

**B) It compares the pointer addresses of `str1` and `str2`, not their actual character contents**

Since string variables (arrays) decay to pointers in most expressions, `==` compares whether they point to the *same memory location*, not whether the characters they contain are equal. Use `strcmp(str1, str2) == 0` to properly test content equality.
</details>

---

### Q3. Why does `scanf("%s", name);` (without `&`) work correctly for a `char name[50];` array, given that `scanf` normally requires `&` before scalar variables?

A) It is actually a bug and does not work
B) Arrays automatically decay into a pointer to their first element, which is exactly the address `scanf` needs
C) `%s` is a special case that never requires an address
D) `scanf` reads strings differently and does not use addresses at all

<details>
<summary><b>Answer</b></summary>

**B) Arrays automatically decay into a pointer to their first element, which is exactly the address `scanf` needs**

`name` (an array) already evaluates to the address of its first character, so no explicit `&` is needed (and adding one, `&name`, would actually give the wrong type — a pointer to the whole array rather than to its first char).
</details>

---

### Q4. What does `strcmp("apple", "banana")` return (in terms of sign, not exact value)?

A) A positive number
B) Exactly `0`
C) A negative number
D) It is undefined and compiler-dependent

<details>
<summary><b>Answer</b></summary>

**C) A negative number**

`strcmp` compares strings character by character lexicographically. Since `'a'` (in "apple") comes before `'b'` (in "banana") in ASCII order, `strcmp` returns a negative value, indicating the first string precedes the second alphabetically.
</details>

---

### Q5. Why is `scanf("%s", ...)` unsuitable for reading a full sentence like `"Good Morning"` into a single string variable?

A) `scanf("%s", ...)` can only read numbers
B) `scanf("%s", ...)` stops reading at the first whitespace character, so it would only capture `"Good"`
C) `scanf` cannot be used with character arrays
D) `scanf("%s", ...)` would cause a compilation error for multi-word input

<details>
<summary><b>Answer</b></summary>

**B) `scanf("%s", ...)` stops reading at the first whitespace character, so it would only capture `"Good"`**

`%s` in `scanf` treats any whitespace (space, tab, newline) as a delimiter marking the end of the current "word." To read an entire line including embedded spaces, `fgets()` should be used instead.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain the role of the null character `'\0'` in a C string. What would go wrong if it were missing or overwritten?

<details>
<summary><b>Model Answer</b></summary>

The null character `'\0'` (ASCII value 0) marks the **logical end** of a string's actual content within its underlying `char` array. Every string-handling function (`printf("%s", ...)`, `strlen`, `strcpy`, `strcmp`, etc.) relies on scanning forward until it finds this terminator to know where the meaningful data ends — the array's *physical* size (its declared capacity) is irrelevant to these functions; only the position of `'\0'` matters.

If `'\0'` is missing (e.g., overwritten by other data, or never written in the first place), string functions will keep reading **past the intended end of the string**, continuing through whatever memory happens to follow — producing garbage output, unpredictable behaviour, or even a crash, since there is no other signal telling the function where to stop.
</details>

---

### Q2. Compare `scanf("%s", ...)` and `fgets(...)` for reading string input. When would you prefer each?

<details>
<summary><b>Model Answer</b></summary>

`scanf("%s", buffer)` reads a single "token" — characters up to (but not including) the first whitespace character (space, tab, or newline) — and does **not** consume the trailing whitespace itself. It is convenient for reading single words (e.g., a username with no spaces) but cannot capture multi-word input in one call.

`fgets(buffer, size, stdin)` reads an entire line, **including embedded spaces**, up to either a newline character or `size-1` characters (whichever comes first), and — unlike `scanf("%s")` — it **does** include the trailing `'\n'` in the buffer if there was room, which callers often need to strip off manually afterward.

**Preference:** Use `scanf("%s", ...)` for simple, single-word input; use `fgets(...)` whenever the input may contain spaces (e.g., full names, sentences) or when more robust, size-limited reading is desired.
</details>

---

### Q3. Why is `strcpy()` considered potentially dangerous? Describe the specific risk and one general strategy to mitigate it.

<details>
<summary><b>Model Answer</b></summary>

`strcpy(dest, src)` copies characters from `src` into `dest` **without ever checking whether `dest` is large enough** to hold all of `src`'s characters plus the terminating `'\0'`. If `src` is longer than `dest`'s allocated capacity, `strcpy` will happily keep writing past the end of `dest`'s memory — a classic **buffer overflow**, which can silently corrupt adjacent variables, crash the program, or (in security-sensitive contexts) be exploited by an attacker to overwrite critical memory and potentially execute malicious code.

**Mitigation strategy:** Always ensure the destination buffer is provably large enough for the maximum possible source length before calling `strcpy` (e.g., by sizing buffers generously and validating input length), or use a bounded alternative like `strncpy(dest, src, destSize - 1)` combined with manually ensuring null-termination, which limits how many characters can be written regardless of `src`'s actual length.
</details>

---

### Q4. Trace the following program and give its output.

```c
#include <stdio.h>
#include <string.h>
int main()
{
    char a[20] = "Hello";
    char b[] = "World";

    strcat(a, ", ");
    strcat(a, b);
    strcat(a, "!");

    printf("%s\n", a);
    printf("Length = %zu\n", strlen(a));
    return 0;
}
```

<details>
<summary><b>Model Answer</b></summary>

**Output:**
```
Hello, World!
Length = 13
```

Trace: `a` starts as `"Hello"` (5 chars, in a 20-byte buffer). Successive `strcat` calls append `", "` (making `"Hello, "`), then `"World"` (making `"Hello, World"`), then `"!"` (making `"Hello, World!"`). The final string has 13 visible characters (`H-e-l-l-o-,-space-W-o-r-l-d-!`), so `strlen` reports `13` (excluding the terminating `'\0'`, which `strlen` never counts).
</details>

---

### Q5. Explain why a string, like any array, can be traversed using either index notation (`str[i]`) or pointer notation (`*p`, incrementing `p`). Write both versions of a loop that prints each character of `str` on its own line.

<details>
<summary><b>Model Answer</b></summary>

A C string is stored as a `char` array, and — just like any other array discussed in Chapter 13 — its name decays to a pointer to its first element in most expressions. This means both indexing and pointer arithmetic access exactly the same underlying memory, just expressed differently.

**Index-based version:**
```c
int i;
for (i = 0; str[i] != '\0'; i++)
    printf("%c\n", str[i]);
```

**Pointer-based version:**
```c
char *p = str;
while (*p != '\0')
{
    printf("%c\n", *p);
    p++;
}
```

Both loops stop at the same condition (`'\0'` reached) and visit exactly the same sequence of characters — the choice between them is purely stylistic/contextual, since `str[i]` and `*(str+i)` (which is what advancing `p` effectively does) are equivalent.
</details>
