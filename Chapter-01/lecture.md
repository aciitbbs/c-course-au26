# Chapter 1 — Getting Started

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 1: "Getting Started"

---

## 1.1 What is C?

C is a **general-purpose, procedural, compiled programming language** created by **Dennis Ritchie** at Bell Telephone Laboratories (AT&T) in **1972**. It was developed alongside the UNIX operating system, most of which is itself written in C.

### Why C is Still Taught and Used

| Reason | Explanation |
|---|---|
| **Foundation language** | C++, Java, C#, Python, JavaScript, Rust — all borrow syntax or runtime concepts from C |
| **Systems programming** | Operating system kernels, device drivers, embedded firmware are written in C |
| **Performance** | Very thin abstraction over the hardware; minimal runtime overhead |
| **Portability** | The same C source can be compiled for almost any processor/OS combination |
| **Pedagogical value** | Teaches you *how memory, pointers, and function calls actually work* — knowledge every engineer needs |

### A Brief History

| Year | Milestone |
|---|---|
| 1960 | ALGOL — committee-designed language, influenced structured programming |
| 1963 | CPL (Combined Programming Language), Cambridge/London |
| 1967 | BCPL (Basic CPL), Martin Richards |
| 1970 | B language, Ken Thompson, Bell Labs |
| **1972** | **C language, Dennis Ritchie, Bell Labs** |
| 1978 | *The C Programming Language* (K&R C) published |
| 1989/1990 | ANSI C / ISO C standard — "C89"/"C90" |
| 1999 | C99 standard (`//` comments, variable-length arrays, etc.) |
| 2011 | C11 standard |
| 2018 | C17 standard |

## 1.2 Which C are We Learning?

Different compilers (GCC, Clang, MSVC, Turbo C) support different C standards. This course uses **GCC** targeting the **C99/C11** standard, which is what virtually every modern compiler, IDE, and online judge supports. Concepts in *Let Us C* apply across all standards unless explicitly noted (e.g., `//` single-line comments were formally added in C99).

## 1.3 Getting Started with C — The Character Set

Every C program is written using a fixed set of characters:

| Category | Characters |
|---|---|
| Letters | `A`–`Z`, `a`–`z` |
| Digits | `0`–`9` |
| Special symbols | `~ ! @ # % ^ & * ( ) _ - + = { } [ ] \ \| : ; " ' < > , . ? /` |
| White space | blank, tab (`\t`), newline (`\n`) |

### 1.3.1 Alphabets, Digits, and Special Symbols

C uses these characters to build every valid program element — identifiers, keywords, constants, operators, and punctuation. Nothing outside this set is legal in C source code (except inside string/character literals and comments).

### 1.3.2 Constants, Variables and Keywords

- A **constant** is a value that cannot change during program execution — e.g. `10`, `3.14`, `'A'`.
- A **variable** is a name given to a memory location that *can* change during execution — e.g. `int marks;`.
- A **keyword** is a word that has a standard, pre-defined meaning known to the compiler — e.g. `int`, `while`, `return`. Keywords **cannot** be used as identifiers (variable/function names).

### 1.3.3 Types of Constants

| Constant Type | Example | Description |
|---|---|---|
| Integer constant | `123`, `-7`, `0` | Whole number, no decimal point |
| Real (floating-point) constant | `3.14`, `-0.5`, `2.0` | Has a decimal point (or exponent) |
| Character constant | `'A'`, `'9'`, `'$'` | A single character enclosed in single quotes |
| String literal | `"Hello"` | Sequence of characters in double quotes (technically not a "constant" of a scalar type, but treated as one for I/O purposes) |

### 1.3.4 Rules for Constructing Integer Constants

1. An integer constant must have at least one digit.
2. It must not have a decimal point.
3. It can be preceded by an optional `+` or `-` sign; if no sign, it is assumed positive.
4. No commas or blanks are allowed within the constant.
5. It must lie within the range of representable integer values on the machine (typically `-2147483648` to `2147483647` for a 32-bit `int`).
6. C also allows **octal** constants (leading `0`, e.g. `013`) and **hexadecimal** constants (leading `0x`/`0X`, e.g. `0x1F`).

```c
int decimalNum = 25;
int octalNum   = 031;   /* interpreted in base 8 → 25 in decimal */
int hexNum     = 0x19;  /* interpreted in base 16 → 25 in decimal */
```

### 1.3.5 Rules for Constructing Real Constants

Real constants (also called floating-point constants) can be written in two forms:

**Fractional form:**
1. Must have at least one digit.
2. Must have a decimal point.
3. Can be positive or negative; default is positive.
4. No commas or blanks are allowed.

```c
float pi = 3.14159;
float negative = -0.005;
```

**Exponential form** (used for very large/small numbers):

```c
float avogadro = 6.02e23;   /* 6.02 × 10^23 */
float electron = 1.6e-19;   /* 1.6 × 10^-19 */
```

- The mantissa and the exponent are separated by `e` or `E`.
- The mantissa must have at least one digit and may have a sign.
- The exponent must be an integer, may have a sign.

### 1.3.6 Rules for Constructing Character Constants

1. A character constant is a single alphabet, digit, or special symbol enclosed in **single quotes**.
2. Both quotes must point to the left (standard single quote character `'`).
3. Internally, C stores character constants as their **ASCII (integer) codes** — e.g., `'A'` is stored as `65`, `'a'` as `97`, `'0'` as `48`.

```c
char grade = 'A';
printf("%d\n", grade);   /* prints 65 — the ASCII code of 'A' */
```

### 1.3.7 Types of C Variables and Data Types

| Data Type | Typical Size | Typical Range | Format Specifier |
|---|---|---|---|
| `char` | 1 byte | -128 to 127 | `%c` (as character), `%d` (as integer) |
| `int` | 4 bytes | -2,147,483,648 to 2,147,483,647 | `%d` |
| `float` | 4 bytes | ~3.4E-38 to 3.4E+38 (~6 significant digits) | `%f` |
| `double` | 8 bytes | ~1.7E-308 to 1.7E+308 (~15 significant digits) | `%lf` |

> Sizes are compiler/platform dependent in principle, but these are the near-universal values on modern 32-bit and 64-bit systems using GCC. You can always verify with the `sizeof` operator (covered in later chapters).

### 1.3.8 Rules for Constructing Variable Names

1. A variable name is any combination of 1 to 31 (or more, but only first 31 are guaranteed significant in old standards) alphabets, digits, or underscores.
2. The first character must be an **alphabet** or an **underscore** (`_`).
3. No commas or blanks are allowed within a variable name.
4. No special symbol other than an underscore can be used.
5. C is **case-sensitive**: `total`, `Total`, and `TOTAL` are three distinct variables.
6. A keyword cannot be used as a variable name.

| Valid ✅ | Invalid ❌ | Reason for Invalidity |
|---|---|---|
| `student_name` | `1st_year` | Cannot start with a digit |
| `_count` | `roll number` | Contains a space |
| `totalMarks` | `int` | `int` is a reserved keyword |
| `x1` | `marks@iit` | `@` is not allowed |

### 1.3.9 C Keywords

C (per the ANSI/C89 standard used as the pedagogical baseline in *Let Us C*) has **32 keywords**:

```
auto      break     case      char      const     continue
default   do        double    else      enum      extern
float     for       goto      if        int       long
register  return    short     signed    sizeof    static
struct    switch    typedef   union     unsigned  void
volatile  while
```

These words are reserved by the language and always written in lowercase. They cannot be redefined or used as identifiers.

## 1.4 The First C Program

Every C program has a well-defined skeleton. Let's dissect a minimal example:

```c
/* My First C Program            */
/* Prints a welcome message      */

#include <stdio.h>          /* Step 1: Preprocessor directive */

int main()                   /* Step 2: main() — the entry point */
{
    /* Step 3: Variable declaration */
    int rollNumber = 101;

    /* Step 4: Executable statements */
    printf("Welcome to IIT Bhubaneswar!\n");
    printf("Roll Number: %d\n", rollNumber);

    return 0;                /* Step 5: Return statement */
}
```

**Output:**
```
Welcome to IIT Bhubaneswar!
Roll Number: 101
```

### 1.4.1 Form of a C Program

A typical C program's overall shape is:

```
documentation section        (comments describing the program)
link section                 (#include statements)
definition section           (#define macros, if any)
global declaration section   (global variables, if any)
main() function section
{
     declaration part;
     executable part;
}
subprogram section            (user-defined functions, if any)
```

### 1.4.2 Comments in a C Program

```c
/* This is a
   multi-line comment */

// This is a single-line comment (C99 onwards, supported by all modern compilers)
```

- Comments are completely ignored by the compiler — they exist purely for human readers.
- Comments **cannot be nested** in standard C (`/* /* ... */ */` is invalid — the first `*/` ends the comment).
- Use comments to explain *why*, not just *what* — good commenting is a hallmark of professional code.

### 1.4.3 What is `main( )`?

- `main()` is a **function** — a self-contained block of code that performs a task.
- Every executable C program must have **exactly one** `main()` function; execution always begins there, regardless of where it is physically placed in the file.
- The keyword `int` before `main` states that this function returns an integer value to the operating system upon completion.
- The parentheses `()` (empty here) indicate the parameter list — this version of `main` takes no arguments (later, in Chapter 20, you will see `main(int argc, char *argv[])`).
- The braces `{ }` enclose the **body** of the function.

### 1.4.4 Variables and their Usage

```c
int rollNumber;        /* declaration: reserve memory of size 4 bytes, name it rollNumber */
rollNumber = 101;       /* assignment: store a value in that memory */
int marks = 95;         /* declaration + initialization in a single statement */
```

An uninitialized local variable contains a **garbage value** (whatever bits happened to already be in that memory). Always initialize variables before you read them.

### 1.4.5 `printf( )` and its Purpose

`printf()` is a **library function** (declared in `stdio.h`) used to display formatted output on the screen.

```c
printf("format string", arg1, arg2, ...);
```

| Format Specifier | Meaning |
|---|---|
| `%d` / `%i` | signed decimal integer |
| `%f` | floating point (`float`, default 6 decimal digits) |
| `%c` | single character |
| `%s` | string (null-terminated char array) |
| `%lf` | `double` |
| `%%` | prints a literal `%` |

Escape sequences frequently used inside string literals:

| Sequence | Meaning |
|---|---|
| `\n` | newline |
| `\t` | horizontal tab |
| `\\` | literal backslash |
| `\"` | literal double quote |
| `\0` | null character (string terminator) |

### 1.4.6 Compilation and Execution

```
┌─────────────┐     ┌──────────────┐     ┌──────────┐     ┌────────┐     ┌────────────┐
│ Source Code │────▶│ Preprocessor │────▶│ Compiler │────▶│ Linker │────▶│ Executable │
│  (.c file)  │     │ (expands #)  │     │ (→ .o)   │     │        │     │  (a.out)   │
└─────────────┘     └──────────────┘     └──────────┘     └────────┘     └────────────┘
```

| Stage | What Happens | Input → Output |
|---|---|---|
| Preprocessing | Expands `#include`/`#define`, strips comments | `.c` → translated source |
| Compilation | Translates C to assembly/object code, checks syntax | → `.o` object file |
| Linking | Resolves calls to library functions (`printf`, etc.), combines object files | `.o` → executable |

Compiling and running on the command line:

```bash
gcc program.c -o program      # compile + link into "program"
./program                     # run on Linux/macOS
program.exe                   # run on Windows
```

- `gcc` — the GNU C Compiler.
- `-o program` — names the resulting executable `program` (otherwise it defaults to `a.out`).
- Adding `-Wall -Wextra` enables extra warnings that catch common bugs before runtime: `gcc -Wall -Wextra program.c -o program`.

## 1.5 Receiving Input

The `scanf()` function reads formatted input typed at the keyboard.

```c
scanf("format string", &variable1, &variable2, ...);
```

> **Critical rule:** The address-of operator `&` is **mandatory** before each scalar variable name in `scanf()`. It tells `scanf()` *where in memory* to place the value typed by the user. Forgetting `&` is one of the most common beginner bugs, and can crash your program or corrupt memory.

```c
#include <stdio.h>

int main()
{
    int age;
    float height;

    printf("Enter your age: ");
    scanf("%d", &age);

    printf("Enter your height (in cm): ");
    scanf("%f", &height);

    printf("You are %d years old and %.1f cm tall.\n", age, height);
    return 0;
}
```

**Sample run:**
```
Enter your age: 18
Enter your height (in cm): 175.5
You are 18 years old and 175.5 cm tall.
```

## 1.6 A Complete Worked Example

```c
#include <stdio.h>

int main()
{
    float radius, area, circumference;
    const float PI = 3.14159f;

    printf("Enter the radius of the circle: ");
    scanf("%f", &radius);

    area = PI * radius * radius;
    circumference = 2 * PI * radius;

    printf("\nCircle Details\n");
    printf("Radius        = %.2f\n", radius);
    printf("Area          = %.4f\n", area);
    printf("Circumference = %.4f\n", circumference);

    return 0;
}
```

**Sample run:**
```
Enter the radius of the circle: 7.0

Circle Details
Radius        = 7.00
Area          = 153.9380
Circumference = 43.9823
```

## 1.7 Common Beginner Mistakes

| Mistake | Example | Fix |
|---|---|---|
| Missing semicolon | `int x = 5` | `int x = 5;` |
| Forgetting `&` in `scanf` | `scanf("%d", age);` | `scanf("%d", &age);` |
| Using `=` instead of the format string properly | `printf(rollNumber);` | `printf("%d", rollNumber);` |
| Case mismatch | `Main()` instead of `main()` | C is case-sensitive; use `main()` |
| Using a keyword as identifier | `int float = 5;` | Rename the variable |
| Uninitialized variable use | `int x; printf("%d", x);` | Initialize `x` before use |

## 1.8 Key Takeaways

1. Every C program begins execution at `main()`.
2. `#include <stdio.h>` brings in declarations for `printf`/`scanf`.
3. Every executable statement ends with a semicolon `;`; preprocessor directives (`#include`, `#define`) do **not**.
4. Variables must be declared with a data type before use; uninitialized variables hold garbage values.
5. `printf()` writes output; `scanf()` reads input — and `scanf()` needs `&` before scalar variable names.
6. C has exactly 32 keywords; these cannot be used as identifiers.
7. A C program is compiled in three broad stages: preprocessing → compilation → linking.
