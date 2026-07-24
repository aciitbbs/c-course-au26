# Chapter 4 — Quiz: More Complex Decision Making

## 📖 Topics Covered: Logical operators (`&&`, `\|\|`, `!`), short-circuit evaluation, `else if` ladder, ternary operator

---

## Part A: Multiple Choice Questions (5)

### Q1. What is the output of `printf("%d", 5 && 0);`?

A) `5`
B) `0`
C) `1`
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**B) `0`**

`&&` requires both operands to be non-zero (true) for the result to be `1`. Since `0` (the second operand) is false, `5 && 0` evaluates to `0`, regardless of the first operand's actual value.
</details>

---

### Q2. Given `int x = 0;`, what happens when evaluating `if (x != 0 && 10 / x > 2)`?

A) Program crashes due to division by zero
B) The condition is false, and `10/x` is never evaluated, due to short-circuiting
C) `10/x` evaluates to `0`
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**B) The condition is false, and `10/x` is never evaluated, due to short-circuiting**

Since `x != 0` is false, `&&` short-circuits: C guarantees the right-hand operand is not evaluated when the left-hand operand of `&&` is already false, so the potential division-by-zero is safely avoided.
</details>

---

### Q3. What is the value of `larger` after `int larger = (7 > 12) ? 7 : 12;`?

A) `7`
B) `12`
C) `1`
D) `0`

<details>
<summary><b>Answer</b></summary>

**B) `12`**

`(7 > 12)` is false, so the ternary operator evaluates to the value after the colon, `12`.
</details>

---

### Q4. Which expression correctly checks that `n` lies strictly between 1 and 100 (exclusive)?

A) `n > 1 & n < 100`
B) `n > 1 && n < 100`
C) `n > 1 | n < 100`
D) `n > 1, n < 100`

<details>
<summary><b>Answer</b></summary>

**B) `n > 1 && n < 100`**

`&&` is the logical AND operator, correctly requiring both bounds to hold simultaneously. `&` is the bitwise AND (different operator, different semantics), `|` is bitwise OR, and a comma just separates two independent expressions without logical combination.
</details>

---

### Q5. In an `else if` ladder checking grade boundaries in decreasing order (`>=90`, then `>=75`, then `>=60`...), what happens if a student scores exactly 90?

A) Only the `>=90` branch executes; later branches are skipped
B) All matching branches execute
C) Only the last branch (`else`) executes
D) Compilation error

<details>
<summary><b>Answer</b></summary>

**A) Only the `>=90` branch executes; later branches are skipped**

An `else if` ladder stops at the *first* true condition (evaluated top to bottom) and skips all subsequent conditions in the chain, regardless of whether they would also be true.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain "short-circuit evaluation" for `&&` and `||`, and give a practical example of why it is useful.

<details>
<summary><b>Model Answer</b></summary>

Short-circuit evaluation means C does not evaluate the second operand of `&&` or `||` if the result can already be determined from the first operand alone:

- For `A && B`: if `A` is false, the whole expression must be false, so `B` is skipped entirely.
- For `A || B`: if `A` is true, the whole expression must be true, so `B` is skipped entirely.

**Practical use — guarding against a crash:**
```c
if (ptr != NULL && *ptr > 0)   /* if ptr is NULL, "*ptr" is never evaluated, avoiding a crash */
```
Here, if `ptr` is `NULL`, dereferencing it (`*ptr`) would be unsafe. Because `&&` short-circuits, `*ptr` is only evaluated once we already know `ptr` is not `NULL`.
</details>

---

### Q2. Rewrite the following nested `if-else` using an `else if` ladder, and explain why the ladder version is more readable.

```c
if (marks >= 90) {
    printf("A");
} else {
    if (marks >= 75) {
        printf("B");
    } else {
        if (marks >= 60)
            printf("C");
        else
            printf("F");
    }
}
```

<details>
<summary><b>Model Answer</b></summary>

```c
if (marks >= 90)
    printf("A");
else if (marks >= 75)
    printf("B");
else if (marks >= 60)
    printf("C");
else
    printf("F");
```

Both versions are logically identical (the `else if` form is exactly the nested form written on successive lines). The ladder form is more readable because it avoids deepening indentation with every additional condition, making a long sequence of mutually-exclusive range checks easy to scan top-to-bottom.
</details>

---

### Q3. What is the ternary (conditional) operator? Why is it called "ternary," and what are its restrictions compared to a full `if-else`?

<details>
<summary><b>Model Answer</b></summary>

The ternary operator `?:` is C's only operator that takes **three** operands — `condition ? valueIfTrue : valueIfFalse` — hence the name "ternary" (from Latin *ternarius*, "consisting of three").

Unlike `if-else`, which is a *statement* that can contain arbitrarily many statements in each branch, the ternary operator is an **expression** that must produce a single value usable wherever an expression is expected (e.g., inside `printf`, or on the right side of `=`). It is best suited to short, simple two-way value choices; using it for complex logic with side effects reduces readability and is discouraged.

```c
int max = (a > b) ? a : b;   /* concise and clear */
```
</details>

---

### Q4. Using De Morgan's laws, simplify `!(age >= 18 && hasID)` into an equivalent expression without a leading `!` over the whole compound condition.

<details>
<summary><b>Model Answer</b></summary>

De Morgan's law states `!(A && B)` is equivalent to `(!A || !B)`. Applying it:

```c
!(age >= 18 && hasID)
   ≡  (!(age >= 18)) || (!hasID)
   ≡  (age < 18) || (!hasID)
```

So `!(age >= 18 && hasID)` is logically equivalent to `age < 18 || !hasID` — "either underage, or does not have an ID."
</details>

---

### Q5. Trace the output of the following program for input `ch = 'e'`.

```c
#include <stdio.h>
int main()
{
    char ch;
    scanf(" %c", &ch);

    if (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u')
        printf("Vowel\n");
    else
        printf("Consonant\n");
    return 0;
}
```

<details>
<summary><b>Model Answer</b></summary>

**Output:** `Vowel`

Trace: The condition checks each equality with `||` in sequence. `ch == 'a'` is false (`ch` is `'e'`), so evaluation proceeds to `ch == 'e'`, which is **true**. Because `||` short-circuits once a true operand is found, the remaining comparisons (`'i'`, `'o'`, `'u'`) are never evaluated, and the whole condition is true, so `Vowel` is printed.
</details>
