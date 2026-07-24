# Chapter 3 — Quiz: Decision Control Instruction

## 📖 Topics Covered: `if`, `if-else`, nested `if-else`, dangling else, common pitfalls

---

## Part A: Multiple Choice Questions (5)

### Q1. What is the output of the following code?

```c
int marks = 35;
if (marks = 40)
    printf("PASS");
else
    printf("FAIL");
```

A) `FAIL`
B) `PASS`
C) Compilation error
D) No output

<details>
<summary><b>Answer</b></summary>

**B) `PASS`**

`marks = 40` is an *assignment*, not a comparison. It assigns `40` to `marks` and the expression's value becomes `40`, which is non-zero (true), so `PASS` is printed. This is the classic `=` vs `==` bug.
</details>

---

### Q2. In the following nested code, which `if` does the `else` bind to?

```c
if (a > b)
    if (a > c)
        printf("X");
else
    printf("Y");
```

A) The outer `if (a > b)`
B) The inner `if (a > c)`
C) Both, simultaneously
D) Neither — this is a syntax error

<details>
<summary><b>Answer</b></summary>

**B) The inner `if (a > c)`**

An `else` always pairs with the nearest preceding unmatched `if`, regardless of indentation. Here, that is `if (a > c)`, not the outer `if (a > b)` — this is the "dangling else" problem.
</details>

---

### Q3. What does the following code print?

```c
int x = 5;
if (x > 0);
    printf("Positive\n");
```

A) Nothing is printed
B) `Positive` is printed, unconditionally
C) Compilation error
D) `Positive` is printed only if `x > 0`

<details>
<summary><b>Answer</b></summary>

**B) `Positive` is printed, unconditionally**

The semicolon right after `if (x > 0)` forms an empty statement — that becomes the entire "then" branch of the `if`. The `printf` on the next line is a completely separate, unconditional statement.
</details>

---

### Q4. Which of these correctly groups multiple statements under a single `if`?

A) `if (x > 0) printf("a"); printf("b");`
B) `if (x > 0): printf("a"); printf("b");`
C) `if (x > 0) { printf("a"); printf("b"); }`
D) `if (x > 0) then printf("a"); printf("b");`

<details>
<summary><b>Answer</b></summary>

**C) `if (x > 0) { printf("a"); printf("b"); }`**

C requires curly braces `{ }` to group multiple statements as a single block controlled by `if`. Option A only binds the first `printf` to the `if`; option B and D are not valid C syntax at all.
</details>

---

### Q5. What is printed by this program if the user enters `year = 1900`?

```c
if (year % 4 == 0) {
    if (year % 100 == 0) {
        if (year % 400 == 0)
            printf("Leap");
        else
            printf("Not Leap");
    } else {
        printf("Leap");
    }
} else {
    printf("Not Leap");
}
```

A) `Leap`
B) `Not Leap`
C) No output
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**B) `Not Leap`**

`1900 % 4 == 0` is true, and `1900 % 100 == 0` is also true, so we go deeper and check `1900 % 400 == 0`, which is **false** (1900/400 = 4.75). So the innermost `else` executes, printing `Not Leap` — matching the real-world leap year rule (century years must be divisible by 400).
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Differentiate between `if` and `if-else` statements. Illustrate with a short example each.

<details>
<summary><b>Model Answer</b></summary>

An **`if`** statement executes a statement/block only when its condition is true; if the condition is false, nothing extra happens and control simply moves to the statement after the `if`.

```c
if (marks >= 40)
    printf("Passed\n");
```

An **`if-else`** statement provides an explicit alternative path: one block executes if the condition is true, and a *different* block executes if it is false. Exactly one of the two blocks always runs.

```c
if (marks >= 40)
    printf("Passed\n");
else
    printf("Failed\n");
```
</details>

---

### Q2. What is the "dangling else" problem? Explain with an example, and show how to resolve the ambiguity using braces.

<details>
<summary><b>Model Answer</b></summary>

The "dangling else" problem arises when a nested `if` structure has more `if`s than `else`s, making it visually ambiguous (though not to the compiler) which `if` a given `else` belongs to. The C compiler's rule is: **an `else` always attaches to the nearest, previous, unmatched `if`.**

```c
if (a > b)
    if (a > c)
        printf("a is largest\n");
else
    printf("check again\n");   /* binds to "if (a > c)", NOT "if (a > b)" -- likely unintended */
```

To force the `else` to belong to the outer `if` instead, wrap the inner `if` in braces so it becomes "matched" and no longer eligible to claim the `else`:

```c
if (a > b)
{
    if (a > c)
        printf("a is largest\n");
}
else
    printf("check again\n");   /* now correctly binds to "if (a > b)" */
```
</details>

---

### Q3. Explain why omitting curly braces `{ }` around multiple statements after an `if` is a common source of bugs. Give a concrete buggy example.

<details>
<summary><b>Model Answer</b></summary>

In C, `if (condition) statement;` controls **exactly one statement** — the single statement immediately following it, ending at the first semicolon. If a programmer indents multiple lines expecting all of them to be conditional, but forgets the braces, only the first statement is actually conditional; the rest execute unconditionally, regardless of how they are indented (indentation is purely cosmetic in C).

```c
if (marks >= 40)
    printf("Result: PASS\n");
    printf("Congratulations!\n");   /* looks conditional, but ALWAYS executes */
```

Here, `printf("Congratulations!\n");` runs even if `marks < 40`, because it is not part of the `if`. The fix is to always use `{ }` when more than one statement should be controlled:

```c
if (marks >= 40)
{
    printf("Result: PASS\n");
    printf("Congratulations!\n");
}
```
</details>

---

### Q4. Trace through the following code for `a = 10, b = 20, c = 15` and give the output.

```c
if (a >= b)
{
    if (a >= c)
        printf("%d is the largest.\n", a);
    else
        printf("%d is the largest.\n", c);
}
else
{
    if (b >= c)
        printf("%d is the largest.\n", b);
    else
        printf("%d is the largest.\n", c);
}
```

<details>
<summary><b>Model Answer</b></summary>

**Output:** `20 is the largest.`

Trace: `a >= b` → `10 >= 20` is **false**, so control goes to the `else` block. Inside it, `b >= c` → `20 >= 15` is **true**, so `printf("%d is the largest.\n", b)` executes, printing `20 is the largest.`
</details>

---

### Q5. Why is `if (flag == 1)` generally preferred in student code over `if (flag = 1)`, and what tool/compiler flag can help catch this mistake automatically?

<details>
<summary><b>Model Answer</b></summary>

`if (flag == 1)` correctly *compares* `flag` to `1`, evaluating to true only when `flag` actually equals `1`. `if (flag = 1)` instead *assigns* `1` to `flag` and always evaluates to true (since `1` is non-zero) — silently overwriting the variable's previous value and always taking the "true" branch, which is almost never the programmer's intent.

Compiling with extra warnings enabled, e.g. `gcc -Wall -Wextra program.c`, causes GCC to emit a warning such as *"suggest parentheses around assignment used as truth value"* whenever an assignment is used directly as a condition — this is a good habit that catches this exact bug before runtime.
</details>
