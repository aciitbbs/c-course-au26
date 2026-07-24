# Chapter 16 — Lecture: Handling Multiple Strings

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 16: "Handling Multiple Strings"

---

## 16.1 Two-Dimensional Array of Characters

The most straightforward way to store a **list of strings** (e.g., names of students) is a **2-D `char` array** — conceptually, an array of fixed-length strings:

```c
char names[3][20] = {
    "Alice",
    "Bob",
    "Christopher"
};
```

- Each **row** is one string (up to 19 usable characters + `'\0'`, since each row has 20 bytes total).
- `names[0]` is `"Alice"`, `names[1]` is `"Bob"`, `names[2]` is `"Christopher"`.

```c
#include <stdio.h>
int main()
{
    char names[3][20] = {"Alice", "Bob", "Christopher"};
    int i;

    for (i = 0; i < 3; i++)
        printf("%s\n", names[i]);

    return 0;
}
```

### The Wasted-Space Problem

Every row must be as wide as the **longest** string you intend to store, so shorter strings waste unused memory:

```c
char names[3][20];   /* "Bob" only needs 4 bytes, but its row still reserves 20 */
```

For a large list with widely varying name lengths, this wasted space can add up significantly — motivating the next approach.

## 16.2 Array of Pointers to Strings

A much more memory-efficient alternative stores an **array of `char *` pointers**, each pointing to a separately-sized string (often a string literal, which the compiler stores compactly):

```c
char *names[3] = {
    "Alice",
    "Bob",
    "Christopher"
};
```

Here, each pointer points to a string that occupies **exactly** the memory it needs (length + 1 for `'\0'`), with **no wasted padding**.

```c
#include <stdio.h>
int main()
{
    char *names[3] = {"Alice", "Bob", "Christopher"};
    int i;

    for (i = 0; i < 3; i++)
        printf("%s\n", names[i]);

    return 0;
}
```

### Memory Layout Comparison

| Approach | Declaration | Memory Characteristics |
|---|---|---|
| 2-D `char` array | `char names[3][20];` | One contiguous block; every row fixed-width; simple but can waste space |
| Array of pointers | `char *names[3];` | Each string separately sized/allocated; no wasted padding, but requires an extra pointer per string and the strings may be scattered in memory |

## 16.3 Limitation of Array of Pointers to Strings

While memory-efficient, an **array of pointers initialized with string literals** has an important restriction: string literals in C are typically **read-only** (attempting to modify them is undefined behaviour on many systems, and some compilers/platforms will actively crash the program):

```c
char *names[3] = {"Alice", "Bob", "Christopher"};
names[0][0] = 'X';   /* DANGEROUS: modifying a string literal is undefined behaviour */
```

If you need a **modifiable** collection of strings, either:
1. Use a 2-D `char` array (`char names[3][20];`), where each row is a genuine, writable array, or
2. Dynamically allocate writable memory for each string individually (via `malloc` and `strcpy`, an advanced technique briefly previewed in Chapter 24).

```c
char names[3][20] = {"Alice", "Bob", "Christopher"};
names[0][0] = 'X';   /* SAFE: names[0] is a genuine writable char array, not a literal */
```

## 16.4 Sorting an Array of Strings

Since strings cannot be compared with `<`/`>`/`==` directly (those would compare addresses or cause type errors), sorting a list of strings requires **`strcmp()`** inside the comparison logic:

```c
#include <stdio.h>
#include <string.h>

void sortStrings(char arr[][20], int n)
{
    int i, j;
    char temp[20];

    for (i = 0; i < n - 1; i++)
    {
        for (j = 0; j < n - 1 - i; j++)
        {
            if (strcmp(arr[j], arr[j + 1]) > 0)   /* swap if arr[j] comes AFTER arr[j+1] alphabetically */
            {
                strcpy(temp, arr[j]);
                strcpy(arr[j], arr[j + 1]);
                strcpy(arr[j + 1], temp);
            }
        }
    }
}

int main()
{
    char names[4][20] = {"Charlie", "Alice", "David", "Bob"};
    int i;

    sortStrings(names, 4);

    printf("Sorted names:\n");
    for (i = 0; i < 4; i++)
        printf("%s\n", names[i]);

    return 0;
}
```

**Output:**
```
Sorted names:
Alice
Bob
Charlie
David
```

## 16.5 Worked Programs

### Program 1: Reading and Displaying a List of Names

```c
#include <stdio.h>

int main()
{
    char names[5][30];
    int i, n;

    printf("Enter number of names: ");
    scanf("%d", &n);

    for (i = 0; i < n; i++)
    {
        printf("Enter name %d: ", i + 1);
        scanf("%s", names[i]);
    }

    printf("\nNames entered:\n");
    for (i = 0; i < n; i++)
        printf("%d. %s\n", i + 1, names[i]);

    return 0;
}
```

### Program 2: Searching for a Name in a List of Strings

```c
#include <stdio.h>
#include <string.h>

int findName(char arr[][30], int n, char key[])
{
    int i;
    for (i = 0; i < n; i++)
    {
        if (strcmp(arr[i], key) == 0)
            return i;
    }
    return -1;
}

int main()
{
    char names[4][30] = {"Alice", "Bob", "Charlie", "David"};
    char search[30];

    printf("Enter a name to search: ");
    scanf("%s", search);

    int index = findName(names, 4, search);

    if (index != -1)
        printf("%s found at position %d.\n", search, index + 1);
    else
        printf("%s not found.\n", search);

    return 0;
}
```

## 16.6 Key Takeaways

1. Multiple strings can be stored either as a **2-D `char` array** (fixed-width rows, simple, writable) or an **array of `char *` pointers** (variable-width, memory-efficient, but often read-only if initialized from literals).
2. Modifying string-literal content pointed to by an array of pointers is undefined behaviour — use a 2-D array when the strings must be mutable.
3. Strings must be compared with `strcmp()`, never `<`, `>`, or `==`, when sorting or searching a list of strings.
4. `strcpy()` is required (not `=`) to actually swap/move string content between array rows during sorting.
5. Choose the 2-D array approach for simplicity and mutability; choose the pointer-array approach when memory efficiency for many strings of very different lengths matters more.
