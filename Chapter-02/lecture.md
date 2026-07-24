# Chapter 2 — Lecture: C Instructions

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 2: "C Instructions"

---

## 2.1 Types of Instructions

Once a variable is declared, a C program manipulates it through **instructions**. *Let Us C* classifies C instructions into three broad kinds:

| Instruction Type | Purpose |
|---|---|
| **Type Declaration Instruction** | Declares the type of variables used in a C program |
| **Arithmetic Instruction** | Performs arithmetic operations between constants and variables |
| **Control Instruction** | Controls the sequence of execution of statements (covered from Chapter 3 onwards) |

## 2.2 Type Declaration Instruction

This instruction is used to declare the type of variables being used in the program.

```c
int i;
float a, b, c;
char ch;
```

Points to note:

1. Variables can be declared in any order.
2. In practice, group declarations logically and give variables meaningful names.
3. You may combine declaration with initialization: `int i = 10, j = 20;`.
4. The general form is: `data-type v1, v2, v3, ... ;`

## 2.3 Arithmetic Instruction

An arithmetic instruction consists of a **variable name on the left** of `=` and **variable names and constants combined with operators** on the right.

```c
int_var = int_expr;
float_var = arithmetic_expression;
```

### 2.3.1 Arithmetic Operators

| Operator | Meaning | Example (`a=13, b=4`) | Result |
|---|---|---|---|
| `+` | Addition | `a + b` | `17` |
| `-` | Subtraction | `a - b` | `9` |
| `*` | Multiplication | `a * b` | `52` |
| `/` | Division | `a / b` | `3` (integer division truncates) |
| `%` | Modulus (remainder) | `a % b` | `1` |

> `%` only works with **integer operands**; it is undefined for `float`/`double`.

### 2.3.2 The Types of Arithmetic Instructions

*Let Us C* distinguishes:

- **Integer mode arithmetic instruction:** all operands are integers.
  ```c
  int i, j, k;
  i = 10; j = 3;
  k = i / j;    /* k = 3 -- integer division truncates the fractional part */
  ```
- **Real mode arithmetic instruction:** all operands are real (float/double).
  ```c
  float a = 10.0, b = 3.0, c;
  c = a / b;    /* c = 3.333333 */
  ```
- **Mixed mode arithmetic instruction:** operands are a mix of `int` and `float`/`double`.
  ```c
  int i = 10;
  float b = 3.0, c;
  c = i / b;    /* i is promoted to float first: c = 3.333333 */
  ```

## 2.4 Integer and Float Conversions

When an expression contains a mix of `int` and `float`/`double` operands, C automatically **promotes** the "lower" type to the "higher" type before performing the operation — this is called **implicit type conversion**.

**Hierarchy of promotion** (simplified, lowest to highest for arithmetic purposes): `char`/`short` → `int` → `unsigned int` → `long` → `unsigned long` → `float` → `double` → `long double`.

```c
int a = 5;
float b = 2.0;
float result = a / b;   /* 'a' is promoted to float(5.0) before division: result = 2.5 */
```

**Important pitfall — integer division:**

```c
float avg;
int total = 7, count = 2;

avg = total / count;      /* WRONG: total/count computed as INT division = 3, then stored as 3.0 */
avg = (float) total / count;  /* RIGHT: 3.5 -- casting forces float division */
```

## 2.5 Type Conversion in Assignments

When you assign a value of one type to a variable of another type, C performs an **implicit conversion** to match the destination's type:

```c
int i;
float f = 12.99f;
i = f;        /* i becomes 12 -- fractional part is truncated (NOT rounded) */

float g;
int n = 5;
g = n;        /* g becomes 5.0 -- no data loss going from int to float (usually) */
```

| Conversion | Behaviour |
|---|---|
| `float`/`double` → `int` | Fractional part is **truncated** (discarded), not rounded |
| `int` → `float`/`double` | Value is preserved (widened) |
| `double` → `float` | May lose precision |
| large `int` → `char` | Higher-order bits are discarded (can overflow/wrap) |

### Explicit Type Conversion (Type Casting)

You can force a conversion using the **cast operator**: `(type) expression`.

```c
int marksObtained = 43, maxMarks = 60;
float percentage;

percentage = ((float) marksObtained / maxMarks) * 100;   /* 71.666664 */
```

> Casting `((float) marksObtained / maxMarks)` promotes only `marksObtained` to `float` **before** the division happens, so the division is a real-number division, not an integer one. Had we cast the whole expression `(float)(marksObtained / maxMarks)`, the integer division would already have truncated to `0` before the cast — a classic mistake!

## 2.6 Hierarchy of Operations (Operator Precedence)

When multiple operators appear in an expression, C evaluates them according to a strict **precedence** order (highest to lowest, for the operators seen so far):

| Precedence (high→low) | Operators |
|---|---|
| 1 | `()` (parentheses) |
| 2 | `*`, `/`, `%` |
| 3 | `+`, `-` |
| 4 | `=` (assignment) |

```c
int a = 5 + 3 * 2;      /* * evaluated before + : a = 5 + 6 = 11 */
int b = (5 + 3) * 2;    /* parentheses force + first : b = 16 */
```

## 2.7 Associativity of Operators

When two operators of the **same precedence** appear together, **associativity** decides the order:

- `*`, `/`, `%`, `+`, `-` are all **left-to-right associative**.
- `=` (assignment) is **right-to-left associative**.

```c
int a = 20 / 2 * 5;    /* left-to-right: (20/2)*5 = 10*5 = 50, NOT 20/(2*5)=2 */
int x, y, z;
x = y = z = 10;         /* right-to-left: z=10, then y=z, then x=y -> x=y=z=10 */
```

## 2.8 A Complete Worked Program

```c
#include <stdio.h>

int main()
{
    int marksObtained = 76, maxMarks = 100;
    float percentage;
    int totalStudents = 5, passedStudents = 3;
    float passPercentage;

    /* Mixed-mode arithmetic with explicit cast to avoid integer division */
    percentage = ((float) marksObtained / maxMarks) * 100;
    passPercentage = ((float) passedStudents / totalStudents) * 100;

    printf("Marks Percentage : %.2f%%\n", percentage);
    printf("Pass Percentage  : %.2f%%\n", passPercentage);

    return 0;
}
```

**Output:**
```
Marks Percentage : 76.00%
Pass Percentage  : 60.00%
```

## 2.9 Control Instructions (Preview)

*Let Us C* briefly introduces the idea that instructions are executed **sequentially** by default, and that **control instructions** (covered starting Chapter 3) allow you to:

- **Select** between alternative paths of execution (decision control — `if`, `switch`).
- **Repeat** a set of instructions (loop control — `while`, `for`, `do-while`).
- **Alter** the normal sequence (case control, jumps — `break`, `continue`, `goto`).

## 2.10 Common Pitfalls

| Pitfall | Example | Fix |
|---|---|---|
| Integer division surprise | `avg = total / count;` (both `int`) gives truncated result | Cast one operand to `float`: `(float) total / count` |
| Wrong cast placement | `(float)(a / b)` — division happens first as int, then cast | Cast an operand *before* the division: `(float) a / b` |
| `%` on floats | `x % y` where `x`, `y` are `float` | `%` is only defined for integer types |
| Assuming precedence without parentheses | `a + b * c` misread as `(a+b)*c` | Always use parentheses when unsure for clarity |
| Truncation instead of rounding | `int i = 3.99;` gives `i = 3`, not `4` | Add `0.5` before truncating, or use `round()` from `<math.h>` for proper rounding |

## 2.11 Key Takeaways

1. C instructions fall into type-declaration, arithmetic, and control categories.
2. Integer arithmetic truncates; mixing `int` and `float` triggers automatic promotion.
3. Explicit casting `(type) expr` lets you force a particular conversion — cast *before* the operation you want affected.
4. Operator precedence and associativity together determine the exact evaluation order of an expression; use parentheses generously for clarity.
5. Assignment (`=`) is right-to-left associative, enabling chained assignments like `x = y = z = 0;`.
