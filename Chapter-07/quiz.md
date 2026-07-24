# Chapter 7 — Quiz: Case Control Instruction

## 📖 Topics Covered: `switch-case`, fall-through, `break` in `switch`, `default`, `goto`

---

## Part A: Multiple Choice Questions (5)

### Q1. What is the output of the following code if `x = 2`?

```c
switch (x)
{
    case 1:
        printf("One ");
    case 2:
        printf("Two ");
    case 3:
        printf("Three ");
        break;
    default:
        printf("Other ");
}
```

A) `Two`
B) `Two Three`
C) `One Two Three`
D) `Other`

<details>
<summary><b>Answer</b></summary>

**B) `Two Three`**

Execution jumps to `case 2` (matching `x=2`), prints `"Two "`, and since there is no `break` after it, "falls through" into `case 3`, printing `"Three "` as well, before hitting the `break`.
</details>

---

### Q2. Which data types can the controlling expression of a `switch` statement legally use in standard C?

A) Only `int`
B) `int` and `char` (integral types)
C) `float` and `double`
D) Any type, including strings

<details>
<summary><b>Answer</b></summary>

**B) `int` and `char` (integral types)**

`switch` requires its controlling expression to evaluate to an integral type. Floating-point types and strings cannot be used directly as the `switch` expression or as `case` labels in standard C.
</details>

---

### Q3. What happens if a `switch` statement has no `default` label and the expression matches none of the `case` values?

A) A runtime error occurs
B) Control falls through to the first `case`
C) None of the case blocks execute; control simply continues after the `switch`
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**C) None of the case blocks execute; control simply continues after the `switch`**

`default` is optional. If it's absent and no `case` matches, the entire `switch` body is simply skipped — this is not an error.
</details>

---

### Q4. What is the primary purpose of the `break` statement inside a `switch` case?

A) To break out of the enclosing function
B) To prevent execution from "falling through" into the next case
C) To skip to the next iteration of a loop
D) It has no effect inside `switch`

<details>
<summary><b>Answer</b></summary>

**B) To prevent execution from "falling through" into the next case**

Without `break`, once a matching `case` label is found, execution continues sequentially into subsequent case blocks regardless of their labels, until a `break` or the end of the `switch` is reached.
</details>

---

### Q5. Why is `goto` generally discouraged in modern structured C programming?

A) It is not supported by most compilers
B) It can only jump backward, never forward
C) Overuse leads to hard-to-follow "spaghetti code" that is difficult to debug and maintain
D) It causes a runtime crash every time it is used

<details>
<summary><b>Answer</b></summary>

**C) Overuse leads to hard-to-follow "spaghetti code" that is difficult to debug and maintain**

`goto` performs an unconditional jump that can bypass the normal structured flow (loops, conditionals), making programs significantly harder to reason about, trace, and maintain as they grow — hence structured constructs (`if`, `while`, `for`, `switch`) are preferred wherever possible.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain "fall-through" behaviour in a `switch` statement. Give one example where fall-through is used *intentionally* and beneficially.

<details>
<summary><b>Model Answer</b></summary>

"Fall-through" refers to C's default behaviour of continuing execution into subsequent `case` blocks once a matching label is found, **unless** a `break` statement is encountered. Execution does not automatically stop at the end of a matched case's statements.

**Intentional, beneficial use — grouping multiple case labels together:**
```c
switch (ch)
{
    case 'a': case 'e': case 'i': case 'o': case 'u':
        printf("Vowel\n");
        break;
    default:
        printf("Consonant\n");
}
```
Here, stacking several `case` labels with no statements/`break` between them lets all of them share the same block of code — a clean, idiomatic way to handle "any of these values should do the same thing."
</details>

---

### Q2. Compare `switch-case` with an `if-else-if` ladder. Under what circumstances is `switch` clearly preferable, and when must you use `if-else` instead?

<details>
<summary><b>Model Answer</b></summary>

`switch` is preferable when a **single integral variable/expression** is being compared against **several distinct, known constant values** — such as a menu choice or a day number — because it reads cleanly and communicates "pick one of these options" directly.

`if-else` must be used instead when:
- The test involves **ranges** (e.g., `marks >= 60 && marks < 75`), since `switch` can only test *exact equality*, not ranges.
- The controlling value is a **floating-point** number or requires comparisons across **multiple different variables**.
- Conditions involve **logical combinations** (`&&`, `\|\|`) rather than a single equality test.
</details>

---

### Q3. What is the purpose of the `default` label in a `switch` statement? Is it mandatory? Where is it conventionally placed?

<details>
<summary><b>Model Answer</b></summary>

`default` specifies the code to execute when the `switch` expression's value does not match any of the listed `case` constants — it acts as a catch-all, similar to the final `else` in an `if-else-if` ladder. It is **optional**: if omitted and no case matches, the switch simply does nothing and control moves on to the next statement after the switch. By convention, `default` is placed as the **last** label in the switch body (though C permits placing it anywhere), and it typically does not need a trailing `break` since it is already the last block executed.
</details>

---

### Q4. Identify the bug in the following code and explain what output it actually produces for `choice = 1`. Then fix it.

```c
switch (choice)
{
    case 1:
        printf("Starting Process A\n");
    case 2:
        printf("Starting Process B\n");
        break;
    case 3:
        printf("Starting Process C\n");
        break;
}
```

<details>
<summary><b>Model Answer</b></summary>

**Bug:** There is no `break` statement after `case 1`'s `printf`, so execution unintentionally "falls through" into `case 2`.

**Actual output for `choice = 1`:**
```
Starting Process A
Starting Process B
```
Both lines print, even though only Process A was intended, because control continues into `case 2` after finishing `case 1`, and only stops at the `break` found there.

**Fix:** Add a `break` at the end of `case 1`:
```c
case 1:
    printf("Starting Process A\n");
    break;   /* added */
```
</details>

---

### Q5. Write a short explanation of what `goto` does, and describe one legitimate, still-accepted use case for it in modern C, along with the reason it is otherwise avoided.

<details>
<summary><b>Model Answer</b></summary>

`goto label;` performs an **unconditional jump** of control to the statement marked by `label:` anywhere within the same function, regardless of the normal sequential or nested block structure.

**Legitimate accepted use:** Centralized error/cleanup handling near the end of a function, where multiple different error conditions detected at different points all need to jump to the same cleanup code (e.g., closing files, freeing memory) before returning — avoiding duplicated cleanup code at every failure point.

**Why otherwise avoided:** Because `goto` can jump into or out of loops and conditionals in ways that bypass the natural, readable top-down structure of the code, it is very easy to create "spaghetti code" — logic so tangled with jumps that it becomes extremely difficult for anyone (including the original author) to trace execution paths, debug, or safely modify later. Structured alternatives (`if`, `while`, `for`, `switch`, functions) achieve the same goals far more safely and readably in almost all cases.
</details>
