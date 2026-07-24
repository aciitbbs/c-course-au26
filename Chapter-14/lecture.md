# Chapter 14 — Lecture: Multidimensional Arrays

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 14: "Multidimensional Arrays"

---

## 14.1 Two-Dimensional Arrays

A **2-D array** is an "array of arrays" — conceptually a grid or table with **rows and columns**, ideal for representing matrices, grids, and tabular data.

```c
int matrix[3][4];   /* 3 rows, 4 columns -- a total of 12 int elements */
```

### 14.1.1 Initializing a 2-D Array

```c
int matrix[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};

int m2[2][3] = {1, 2, 3, 4, 5, 6};   /* equivalent flat form -- filled row by row */

int m3[2][3] = { {1, 2}, {4} };      /* partial init: remaining elements become 0 */
```

Accessing and traversing elements uses **two nested loops** — the outer loop for rows, the inner for columns:

```c
#include <stdio.h>
int main()
{
    int matrix[2][3] = { {1, 2, 3}, {4, 5, 6} };
    int i, j;

    for (i = 0; i < 2; i++)
    {
        for (j = 0; j < 3; j++)
            printf("%d ", matrix[i][j]);
        printf("\n");
    }
    return 0;
}
```

**Output:**
```
1 2 3 
4 5 6 
```

### 14.1.2 Memory Map of a 2-D Array

C stores multidimensional arrays in **row-major order** — meaning the entire first row is stored completely, followed immediately by the entire second row, and so on, all in one contiguous block of memory.

```
matrix[2][3] = {{1,2,3},{4,5,6}}

Memory layout (contiguous, row-major):
[1][2][3][4][5][6]
 ↑matrix[0][0]   ↑matrix[1][2]
```

The address of `matrix[i][j]` can be computed as: `base_address + (i * num_columns + j) * sizeof(element_type)`.

### 14.1.3 Pointers and 2-D Arrays

`matrix[i]` (a single index into a 2-D array) itself represents the **address of the `i`-th row** — it behaves like a 1-D array (pointer to its first element), i.e., `matrix[i]` is equivalent to `&matrix[i][0]`.

```c
int matrix[2][3] = { {1, 2, 3}, {4, 5, 6} };

printf("%p\n", matrix[0]);     /* address of row 0, i.e., &matrix[0][0] */
printf("%d\n", *matrix[0]);    /* dereferences to matrix[0][0] -- prints 1 */
printf("%d\n", *(matrix[0] + 1));  /* matrix[0][1] -- prints 2 */
printf("%d\n", *(*(matrix + 1) + 2));  /* matrix[1][2] -- prints 6 */
```

### 14.1.4 Pointer to an Array (Row Pointer)

A pointer that can correctly step through an **entire row at a time** must be declared as a "pointer to an array of N ints":

```c
int matrix[2][3] = { {1, 2, 3}, {4, 5, 6} };
int (*rowPtr)[3] = matrix;   /* rowPtr points to a whole row (array of 3 ints) */

printf("%d\n", rowPtr[0][0]);   /* 1 */
printf("%d\n", rowPtr[1][2]);   /* 6 */
rowPtr++;                        /* advances by an ENTIRE ROW (3 ints), not just 1 int */
```

> **Precedence matters:** `int (*rowPtr)[3]` (a pointer to an array of 3 ints) is very different from `int *rowPtr[3]` (an array of 3 pointers to int) — the parentheses around `*rowPtr` are essential.

### 14.1.5 Passing a 2-D Array to a Function

When passing a 2-D array to a function, the **number of columns must be specified** (the compiler needs it to correctly compute each row's stride in memory); the number of rows can be passed as a separate parameter instead:

```c
#include <stdio.h>

void printMatrix(int mat[][3], int rows)   /* column count (3) is MANDATORY here */
{
    int i, j;
    for (i = 0; i < rows; i++)
    {
        for (j = 0; j < 3; j++)
            printf("%d ", mat[i][j]);
        printf("\n");
    }
}

int main()
{
    int matrix[2][3] = { {1, 2, 3}, {4, 5, 6} };
    printMatrix(matrix, 2);
    return 0;
}
```

## 14.2 Array of Pointers

An **array of pointers** stores multiple addresses, commonly used for jagged/variable-length row structures or arrays of strings (Chapter 16):

```c
int a = 1, b = 2, c = 3;
int *arr[3] = {&a, &b, &c};   /* arr is an array of 3 int-pointers */

printf("%d %d %d\n", *arr[0], *arr[1], *arr[2]);   /* prints: 1 2 3 */
```

## 14.3 Three-Dimensional (3-D) Arrays

A 3-D array extends the same idea with a third dimension — useful for representing, e.g., a stack of matrices or volumetric data:

```c
int cube[2][3][4];   /* 2 "layers", each a 3x4 matrix -- total 24 int elements */

int i, j, k;
for (i = 0; i < 2; i++)
    for (j = 0; j < 3; j++)
        for (k = 0; k < 4; k++)
            cube[i][j][k] = i + j + k;
```

Traversing an N-D array simply requires N nested loops, one per dimension.

## 14.4 Worked Programs

### Program 1: Matrix Addition

```c
#include <stdio.h>

void addMatrices(int a[][3], int b[][3], int result[][3], int rows, int cols)
{
    int i, j;
    for (i = 0; i < rows; i++)
        for (j = 0; j < cols; j++)
            result[i][j] = a[i][j] + b[i][j];
}

int main()
{
    int a[2][3] = { {1, 2, 3}, {4, 5, 6} };
    int b[2][3] = { {7, 8, 9}, {10, 11, 12} };
    int result[2][3];
    int i, j;

    addMatrices(a, b, result, 2, 3);

    printf("Sum Matrix:\n");
    for (i = 0; i < 2; i++)
    {
        for (j = 0; j < 3; j++)
            printf("%d ", result[i][j]);
        printf("\n");
    }
    return 0;
}
```

### Program 2: Matrix Multiplication

```c
#include <stdio.h>

int main()
{
    int a[2][3] = { {1, 2, 3}, {4, 5, 6} };
    int b[3][2] = { {7, 8}, {9, 10}, {11, 12} };
    int result[2][2] = {0};
    int i, j, k;

    for (i = 0; i < 2; i++)
    {
        for (j = 0; j < 2; j++)
        {
            for (k = 0; k < 3; k++)
                result[i][j] += a[i][k] * b[k][j];
        }
    }

    printf("Product Matrix:\n");
    for (i = 0; i < 2; i++)
    {
        for (j = 0; j < 2; j++)
            printf("%d ", result[i][j]);
        printf("\n");
    }
    return 0;
}
```

**Output:**
```
Product Matrix:
58 64 
139 154 
```

### Program 3: Transpose of a Matrix

```c
#include <stdio.h>

int main()
{
    int a[2][3] = { {1, 2, 3}, {4, 5, 6} };
    int transpose[3][2];
    int i, j;

    for (i = 0; i < 2; i++)
        for (j = 0; j < 3; j++)
            transpose[j][i] = a[i][j];

    printf("Transpose:\n");
    for (i = 0; i < 3; i++)
    {
        for (j = 0; j < 2; j++)
            printf("%d ", transpose[i][j]);
        printf("\n");
    }
    return 0;
}
```

## 14.5 Key Takeaways

1. A 2-D array is stored contiguously in **row-major order** — the entire first row precedes the second, and so on.
2. `matrix[i]` refers to the address of row `i`; `matrix[i][j]` is equivalent to `*(*(matrix + i) + j)`.
3. When passing a 2-D array to a function, the column count must be specified in the parameter type; only the row count can safely vary via a separate parameter.
4. A "pointer to an array" (`int (*p)[N]`) correctly steps row-by-row; do not confuse it with an "array of pointers" (`int *p[N]`).
5. Matrix operations (addition, multiplication, transpose) are the classic exercises for mastering nested-loop traversal of 2-D arrays.
