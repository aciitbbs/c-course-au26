# Chapter 2 — Quiz: C Instructions

## 📖 Topics Covered: Type declaration, arithmetic instructions, type conversion, operator precedence & associativity

---

## Part A: Multiple Choice Questions (5)

### Q1. What is the value of `x` after `int x = 17 / 5;`?

A) `3.4`
B) `3`
C) `4`
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**B) `3`**

Both operands are integers, so `/` performs integer division, truncating the fractional part: `17/5 = 3.4` → truncated to `3`.
</details>

---

### Q2. What is the value of `17 % 5`?

A) `3.4`
B) `2`
C) `3`
D) Undefined behaviour

<details>
<summary><b>Answer</b></summary>

**B) `2`**

`%` gives the remainder of integer division: `17 = 5*3 + 2`, so `17 % 5 = 2`.
</details>

---

### Q3. What does the expression `(float) 7 / 2` evaluate to?

A) `3`
B) `3.0`
C) `3.5`
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**C) `3.5`**

The cast `(float)` applies only to `7`, converting it to `7.0` *before* the division. Since one operand is now `float`, the whole division is promoted to floating-point: `7.0 / 2 = 3.5`.
</details>

---

### Q4. What is the output of `printf("%d", 5 + 3 * 2);`?

A) `16`
B) `11`
C) `13`
D) `10`

<details>
<summary><b>Answer</b></summary>

**B) `11`**

`*` has higher precedence than `+`, so `3 * 2` is evaluated first (`6`), then `5 + 6 = 11`.
</details>

---

### Q5. In the statement `x = y = z = 5;`, what determines that `z` is assigned first, then `y`, then `x`?

A) Operator precedence
B) Right-to-left associativity of `=`
C) Left-to-right associativity of `=`
D) The order variables were declared

<details>
<summary><b>Answer</b></summary>

**B) Right-to-left associativity of `=`**

The assignment operator groups right-to-left, so `z = 5` happens first, its result (`5`) is then assigned to `y`, and that result is then assigned to `x`.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain, with an example, why `int avg = total / count;` can give a wrong answer when computing an average, and how to fix it.

<details>
<summary><b>Model Answer</b></summary>

If `total` and `count` are both `int`, the `/` operator performs **integer division**, discarding any fractional part, rather than rounding. For example, if `total = 7` and `count = 2`, `total / count` evaluates to `3` (not `3.5`), and storing this in an `int` (or even a `float`) still gives `3.0`.

**Fix:** Explicitly cast one of the operands to `float` (or `double`) *before* the division so the division itself is performed in floating point:

```c
float avg = (float) total / count;   /* 3.5, correct */
```

Casting the *result* instead — `(float)(total / count)` — does not help, because by then the integer division has already truncated the value.
</details>

---

### Q2. What is the difference between implicit type conversion and explicit type conversion (type casting)? Give one example each.

<details>
<summary><b>Model Answer</b></summary>

- **Implicit type conversion** happens automatically, without any special syntax, whenever operands of different types appear together in an expression or assignment. The compiler promotes/demotes types following built-in rules.
  ```c
  int i = 5;
  float f = i;   /* implicit: i is automatically converted to 5.0 */
  ```
- **Explicit type conversion (casting)** is a deliberate instruction from the programmer, written as `(type) expression`, to force a conversion at a specific point in an expression.
  ```c
  int a = 7, b = 2;
  float result = (float) a / b;   /* explicit cast forces floating-point division */
  ```

The key difference is *control*: casting lets the programmer decide exactly when and where a conversion occurs, which is essential for avoiding bugs like unwanted integer division.
</details>

---

### Q3. State the precedence order (highest to lowest) of the arithmetic and assignment operators covered in this chapter, and explain what "associativity" means.

<details>
<summary><b>Model Answer</b></summary>

**Precedence (high to low):**
1. `()` — parentheses
2. `*`, `/`, `%` — multiplicative operators
3. `+`, `-` — additive operators
4. `=` — assignment

**Associativity** determines the evaluation order when two or more operators of the *same* precedence level appear in an expression. `*`, `/`, `%`, `+`, `-` are left-to-right associative (evaluated left to right), while `=` is right-to-left associative — this is why `a = b = c = 5;` correctly assigns `5` to `c` first, then propagates that value leftwards to `b` and `a`.
</details>

---

### Q4. Predict the output of the following program and justify your answer.

```c
#include <stdio.h>
int main()
{
    int a = 9, b = 4;
    float result1, result2;

    result1 = a / b;
    result2 = (float) a / b;

    printf("%.2f\n", result1);
    printf("%.2f\n", result2);
    return 0;
}
```

<details>
<summary><b>Model Answer</b></summary>

**Output:**
```
2.00
2.25
```

`a / b` is `9 / 4` with both operands `int`, so integer division gives `2` (truncated), which is then stored in the `float` variable `result1` as `2.00`. `(float) a / b` casts `a` to `9.0` before the division, so the division is performed in floating point, giving `2.25`, stored correctly in `result2`.
</details>

---

### Q5. What is mixed-mode arithmetic? Explain the automatic type promotion rule C follows when an `int` and a `float` appear in the same expression.

<details>
<summary><b>Model Answer</b></summary>

**Mixed-mode arithmetic** occurs when an expression combines operands of different data types — most commonly `int` and `float`/`double` together, e.g., `int i = 5; float b = 2.0; float c = i / b;`.

C's automatic promotion rule states that before the operation is performed, the "lower" type operand is temporarily converted ("promoted") to match the "higher" type operand, following the hierarchy roughly: `char`/`short` → `int` → `long` → `float` → `double` → `long double`. So in `i / b` above, `i` (an `int`) is promoted to `5.0` (a `float`) before the division executes, yielding a floating-point result `2.5` rather than a truncated integer result.
</details>
