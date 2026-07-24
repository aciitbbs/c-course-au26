# Mid-Semester Revision Summary

This document serves as a quick cheat sheet for the topics covered in Chapters 1–9 and Chapter 13 (1-D Arrays). For deeper review, see each chapter's own `flashcards.md`.

---

## 1. Format Specifiers & ASCII

| Data Type | Specifier | Notes |
|-----------|-----------|-------|
| `int` | `%d` or `%i` | 4 bytes (typically) |
| `float` | `%f` | 4 bytes |
| `double` | `%lf` | 8 bytes |
| `char` | `%c` | 1 byte (ASCII values: 'A'=65, 'a'=97, '0'=48) |

---

## 2. Operator Precedence (High to Low)

1. `()` (Function calls, grouping)
2. `++`, `--`, `!` (Unary operations, evaluated Right-to-Left)
3. `*`, `/`, `%` (Multiplicative)
4. `+`, `-` (Additive)
5. `<`, `<=`, `>`, `>=` (Relational)
6. `==`, `!=` (Equality)
7. `&&` (Logical AND)
8. `||` (Logical OR)
9. `? :` (Ternary / Conditional, evaluated Right-to-Left)
10. `=`, `+=`, `-=`, etc. (Assignment, evaluated Right-to-Left)

---

## 3. Important Concepts to Remember

### Integer Division vs. Float Division
- `5 / 2` $\rightarrow$ `2` (Integer division)
- `5.0 / 2` $\rightarrow$ `2.5` (Float division)
- `(float)(5 / 2)` $\rightarrow$ `2.0` (Division happens first, then casts to float)

### Pre vs. Post Increment
```c
int a = 5;
int x = a++; // x gets 5, then a becomes 6
int y = ++a; // a becomes 7, then y gets 7
```

### Short-Circuit Evaluation
- `(False) && (Anything)` $\rightarrow$ `Anything` is SKIPPED.
- `(True) || (Anything)` $\rightarrow$ `Anything` is SKIPPED.

---

## 4. Control Structures Syntax

### `if-else`
```c
if (condition) {
    // Code
} else if (other_condition) {
    // Code
} else {
    // Code
}
```

### `switch-case`
```c
switch(integer_or_char) {
    case 1:
        // Code
        break; // Don't forget this!
    default:
        // Code
}
```

### `while` loop (Entry-controlled)
```c
while (condition) {
    // Code
}
```

### `for` loop (Entry-controlled)
```c
for (init; condition; update) {
    // Code
}
```

### `do-while` loop (Exit-controlled)
```c
do {
    // Code
} while (condition); // Semicolon is mandatory!
```

---

## 6. Pointers (Chapter 9) — High Priority

- `&x` → address of `x`. `*p` → value at the address stored in `p` (dereference).
- `int *p;` declares "p is a pointer to int" — `*` here is part of the type, not an operation.
- **Call by reference:** pass `&x` to a function expecting `int *`, so it can modify the caller's `x` via `*paramName = ...`.
- Classic `swap()`:
  ```c
  void swap(int *a, int *b) {
      int temp = *a;
      *a = *b;
      *b = temp;
  }
  /* call as: swap(&x, &y); */
  ```
- `(*n)++` increments the **value** pointed to; `*n++` increments the **pointer itself** (parses as `*(n++)`) — a classic precedence trap.
- Never dereference an uninitialized ("wild") pointer.

---

## 7. Arrays (Chapter 13, 1-D only)

- `int arr[5];` → valid indices `0` to `4`. **C never checks bounds** — out-of-range access is undefined behaviour, not an error.
- Array name decays to a pointer to its first element: `arr` ≡ `&arr[0]`.
- Equivalence: `arr[i]` ≡ `*(arr + i)`; `&arr[i]` ≡ `arr + i`.
- Pointer arithmetic is scaled automatically by `sizeof(element type)` — `p+1` moves to the *next* element, not the next byte.
- Passing to a function: the array decays to a pointer, so the function can modify the caller's array; **always** pass the size as a separate parameter (the function cannot deduce it from the pointer alone).
- Common algorithms to know: linear search, finding min/max, bubble sort.

---

## 8. Functions Syntax

```c
// Prototype
return_type function_name(type param1, type param2);

// Definition
return_type function_name(type param1, type param2) {
    // Body
    return value;
}
```
*Note: If the function takes no parameters, use `void` inside the parentheses. If it returns nothing, use `void` as the return type.*

---

## 9. Common Pitfalls for Tracing Output

1. **Assignment in `if`**: 
   `if (x = 0)` evaluates to `0` (False). `if (x = 10)` evaluates to `10` (True).
2. **Dangling `else`**: 
   An `else` always attaches to the nearest unmatched `if` above it within the same block, regardless of indentation.
3. **Empty Loop Body**: 
   `for (int i=0; i<5; i++);` 
   Notice the semicolon. The loop does nothing 5 times, then `i` becomes `5`.
4. **Scope**:
   A variable declared inside a loop or `if` block (e.g., `for (int i=0;...)`) is destroyed once that block ends. You cannot access `i` outside the loop.
5. **Pointer precedence**: `*n++` vs `(*n)++` — always double-check which one a question actually shows.
6. **Array bounds**: never assume C will catch an off-by-one array access — trace the loop condition (`<` vs `<=`) by hand.
