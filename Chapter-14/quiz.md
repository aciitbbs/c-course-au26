# Chapter 14 — Quiz: Multidimensional Arrays

## 📖 Topics Covered: 2-D array declaration/initialization, row-major memory layout, pointers to 2-D arrays, passing 2-D arrays to functions

---

## Part A: Multiple Choice Questions (5)

### Q1. In what order are elements of a 2-D array stored in memory in C?

A) Column-major order (entire first column, then second column, ...)
B) Row-major order (entire first row, then second row, ...)
C) Random order determined by the compiler
D) Diagonal order

<details>
<summary><b>Answer</b></summary>

**B) Row-major order (entire first row, then second row, ...)**

C stores multidimensional arrays in row-major order: all elements of row 0 are stored contiguously, immediately followed by all elements of row 1, and so on.
</details>

---

### Q2. Given `int matrix[2][3] = {{1,2,3},{4,5,6}};`, what does `matrix[1]` represent?

A) The value `1`
B) The address of the second row, equivalent to `&matrix[1][0]`
C) The value `4`
D) A compilation error, since a single index cannot be used on a 2-D array

<details>
<summary><b>Answer</b></summary>

**B) The address of the second row, equivalent to `&matrix[1][0]`**

A single index into a 2-D array yields the address of that entire row (it behaves like a 1-D array/pointer to the row's first element), not a scalar value.
</details>

---

### Q3. When passing a 2-D array to a function, which dimension must be specified in the function's parameter type?

A) Only the number of rows
B) Only the number of columns
C) Both rows and columns must always be specified
D) Neither — C automatically infers both

<details>
<summary><b>Answer</b></summary>

**B) Only the number of columns**

The compiler needs the column count to correctly compute each row's memory stride (how many elements to skip to get to the next row). The row count can be passed separately as an ordinary `int` parameter, since it does not affect the memory layout calculation.
</details>

---

### Q4. What is the difference between `int (*p)[3]` and `int *p[3]`?

A) They are exactly the same
B) `int (*p)[3]` is a pointer to an array of 3 ints; `int *p[3]` is an array of 3 pointers to int
C) `int (*p)[3]` is invalid syntax
D) `int *p[3]` can only hold `char` pointers

<details>
<summary><b>Answer</b></summary>

**B) `int (*p)[3]` is a pointer to an array of 3 ints; `int *p[3]` is an array of 3 pointers to int**

The parentheses change the meaning entirely: `(*p)` groups the dereference with `p` first, making `p` a single pointer to a 3-element array (useful for stepping row-by-row through a 2-D array). Without parentheses, `p[3]` binds first, making `p` an array of 3 separate `int*` pointers.
</details>

---

### Q5. For `int a[2][3] = {{1,2,3},{4,5,6}};`, what does `*(*(a + 1) + 2)` evaluate to?

A) `3`
B) `4`
C) `6`
D) An address, not a value

<details>
<summary><b>Answer</b></summary>

**C) `6`**

`a + 1` points to row 1 (`{4,5,6}`). `*(a+1)` gives the address of that row's first element (equivalent to `a[1]`, a pointer/array). Adding `2` and dereferencing again reaches the element at column 2 of row 1, i.e., `a[1][2] = 6`.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain "row-major order" and describe, with a small example, how the address of `matrix[i][j]` can be computed from the array's base address.

<details>
<summary><b>Model Answer</b></summary>

**Row-major order** means a 2-D array is stored in memory as a single contiguous block, laid out one complete row after another: all of row 0's elements first, then all of row 1's elements, and so on.

For an array `matrix[ROWS][COLS]` of element type `T`, the address of `matrix[i][j]` is:
```
base_address + (i * COLS + j) * sizeof(T)
```

**Example:** For `int matrix[2][3]` with base address `1000` and `sizeof(int) = 4`:
- `matrix[0][0]` is at `1000 + (0*3+0)*4 = 1000`
- `matrix[1][2]` is at `1000 + (1*3+2)*4 = 1000 + 5*4 = 1020`

This formula shows exactly why the compiler *must* know the column count (`COLS`) to correctly locate any element — without it, this address calculation is impossible.
</details>

---

### Q2. Why can a function's 2-D array parameter safely have a variable number of rows, but not a variable number of columns? Illustrate with a function signature.

<details>
<summary><b>Model Answer</b></summary>

```c
void printMatrix(int mat[][3], int rows);   /* columns fixed at 3; rows passed separately, can vary */
```

The **column count** directly determines the "stride" — how many `int`s to skip in memory to move from one row to the next (`row_stride = COLS * sizeof(int)`). The compiler needs this fixed value at compile time to generate correct address-computation code for `mat[i][j]`. Without it, the compiler would have no way to know where row `1` begins relative to row `0`.

The **row count**, however, does not affect this per-element address formula at all — it only determines how many rows exist in total, which is simply a loop bound that can be freely passed as an ordinary runtime `int` value, since it plays no role in computing any individual element's address.
</details>

---

### Q3. Trace the matrix multiplication algorithm for `a = [[1,2],[3,4]]` (2×2) and `b = [[5,6],[7,8]]` (2×2), computing the resulting 2×2 product matrix by hand.

<details>
<summary><b>Model Answer</b></summary>

Using the standard formula `result[i][j] = Σ a[i][k] * b[k][j]`:

- `result[0][0] = a[0][0]*b[0][0] + a[0][1]*b[1][0] = 1*5 + 2*7 = 5+14 = 19`
- `result[0][1] = a[0][0]*b[0][1] + a[0][1]*b[1][1] = 1*6 + 2*8 = 6+16 = 22`
- `result[1][0] = a[1][0]*b[0][0] + a[1][1]*b[1][0] = 3*5 + 4*7 = 15+28 = 43`
- `result[1][1] = a[1][0]*b[0][1] + a[1][1]*b[1][1] = 3*6 + 4*8 = 18+32 = 50`

**Resulting product matrix:**
```
19 22
43 50
```
</details>

---

### Q4. What is the difference between an "array of pointers" and a genuine 2-D array in terms of memory layout? Give one practical use case for an array of pointers.

<details>
<summary><b>Model Answer</b></summary>

A genuine **2-D array** (`int matrix[3][4];`) is a single, contiguous block of memory holding all `3*4 = 12` elements laid out in row-major order — every row has exactly the same fixed length, and the whole structure is allocated as one unit.

An **array of pointers** (`int *arr[3];`) is instead an array of just 3 *addresses*; each address can point to a **completely separate, independently allocated** block of memory (potentially of different lengths for each pointer) — the "rows" are not necessarily contiguous with each other at all.

**Practical use case:** Arrays of pointers are commonly used to represent **jagged arrays** (rows of differing lengths) or, very commonly, **arrays of strings** — e.g., `char *names[3] = {"Alice", "Bob", "Christopher"};` — where each pointer refers to a separately-sized character array, something a genuine fixed-width 2-D `char` array could not represent efficiently.
</details>

---

### Q5. Explain why `int (*rowPtr)[3] = matrix;` followed by `rowPtr++;` advances `rowPtr` by an entire row, rather than by a single `int`.

<details>
<summary><b>Model Answer</b></summary>

`rowPtr` is declared as a **pointer to an array of 3 ints** (note the parentheses: `(*rowPtr)`, meaning `rowPtr` itself is the pointer, and what it points to is "an array of 3 ints"). Pointer arithmetic in C is always scaled by the size of the type being pointed to — here, that type is "array of 3 ints," whose size is `3 * sizeof(int)`.

Therefore, `rowPtr++` advances the pointer by exactly `3 * sizeof(int)` bytes, which is precisely the size of one full row, landing `rowPtr` at the start of the *next row* rather than merely the next individual `int`. This is different from an ordinary `int *p`, where `p++` would only advance by a single `int`'s worth of bytes.
</details>
