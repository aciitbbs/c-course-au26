# Chapter 10 — Lecture: Recursion

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 10: "Recursion"

---

## 10.1 What is Recursion?

**Recursion** occurs when a function calls **itself**, directly or indirectly, to solve a smaller instance of the same problem. Every correctly designed recursive function needs two essential parts:

1. **Base case:** A condition simple enough to be answered directly, without further recursive calls — this **stops** the recursion.
2. **Recursive case:** The function calls itself with a "smaller" or "simpler" version of the original problem, making measurable progress toward the base case.

```c
long factorial(int n)
{
    if (n == 0)              /* base case */
        return 1;
    return n * factorial(n - 1);   /* recursive case: smaller problem (n-1) */
}
```

> **Without a reachable base case, a recursive function calls itself forever**, eventually crashing with a **stack overflow** — the recursive equivalent of an infinite loop.

## 10.2 How Recursion Works — The Call Stack

Each recursive call creates a **new stack frame** holding that call's own local variables and parameters. Calls "stack up" until the base case is reached, and then each call **returns** in reverse order, resolving its pending arithmetic.

**Tracing `factorial(4)`:**

```
factorial(4) = 4 * factorial(3)
factorial(3) = 3 * factorial(2)
factorial(2) = 2 * factorial(1)
factorial(1) = 1 * factorial(0)
factorial(0) = 1                    <-- base case reached, unwinding begins
factorial(1) = 1 * 1 = 1
factorial(2) = 2 * 1 = 2
factorial(3) = 3 * 2 = 6
factorial(4) = 4 * 6 = 24
```

```
Call stack (growing downward as calls are made):
┌─────────────────┐
│ factorial(4)      │  waiting for factorial(3)
├─────────────────┤
│ factorial(3)      │  waiting for factorial(2)
├─────────────────┤
│ factorial(2)      │  waiting for factorial(1)
├─────────────────┤
│ factorial(1)      │  waiting for factorial(0)
├─────────────────┤
│ factorial(0)      │  returns 1 immediately (base case)
└─────────────────┘
```

## 10.3 Recursion vs. Iteration

| Aspect | Recursion | Iteration (loop) |
|---|---|---|
| Mechanism | Function calls itself | `while`/`for`/`do-while` |
| Memory use | Uses call-stack memory per call | Typically constant memory |
| Code clarity | Often more elegant for naturally recursive problems (trees, Tower of Hanoi) | Often more efficient and equally clear for simple counting |
| Risk | Stack overflow if base case is missing/unreachable | Infinite loop if condition never becomes false |

Any recursive solution can, in principle, be rewritten iteratively (and vice versa) — the choice is about clarity and problem structure, not raw computability.

## 10.4 Worked Programs

### Program 1: Factorial (Direct Recursion)

```c
#include <stdio.h>

long factorial(int n)
{
    if (n == 0)
        return 1;
    return n * factorial(n - 1);
}

int main()
{
    int n;
    printf("Enter n: ");
    scanf("%d", &n);
    printf("%d! = %ld\n", n, factorial(n));
    return 0;
}
```

### Program 2: Sum of First N Natural Numbers

```c
#include <stdio.h>

int sumOfN(int n)
{
    if (n == 0)
        return 0;             /* base case */
    return n + sumOfN(n - 1); /* recursive case */
}

int main()
{
    int n;
    printf("Enter n: ");
    scanf("%d", &n);
    printf("Sum = %d\n", sumOfN(n));
    return 0;
}
```

### Program 3: Fibonacci Sequence

```c
#include <stdio.h>

int fibonacci(int n)
{
    if (n == 0)
        return 0;              /* base case 1 */
    if (n == 1)
        return 1;              /* base case 2 */
    return fibonacci(n - 1) + fibonacci(n - 2);   /* recursive case */
}

int main()
{
    int n, i;
    printf("Enter n: ");
    scanf("%d", &n);

    printf("Fibonacci series: ");
    for (i = 0; i < n; i++)
        printf("%d ", fibonacci(i));
    printf("\n");

    return 0;
}
```

**Sample run (n=8):** `0 1 1 2 3 5 8 13`

> **Efficiency note:** This naive recursive Fibonacci recomputes the same values repeatedly (`fibonacci(5)` calls `fibonacci(4)` and `fibonacci(3)`, but `fibonacci(4)` *also* calls `fibonacci(3)` again, etc.), making it exponentially slow for large `n`. An iterative version (or "memoized" recursion, a more advanced technique) is far more efficient in practice — an important lesson in when recursion is elegant versus when it is costly.

### Program 4: Greatest Common Divisor (Recursive Euclidean Algorithm)

```c
#include <stdio.h>

int gcd(int a, int b)
{
    if (b == 0)
        return a;             /* base case */
    return gcd(b, a % b);     /* recursive case */
}

int main()
{
    int a, b;
    printf("Enter two integers: ");
    scanf("%d %d", &a, &b);
    printf("GCD = %d\n", gcd(a, b));
    return 0;
}
```

### Program 5: Tower of Hanoi

```c
#include <stdio.h>

void towerOfHanoi(int n, char from, char via, char to)
{
    if (n == 0)
        return;                       /* base case: nothing to move */

    towerOfHanoi(n - 1, from, to, via);       /* move n-1 disks out of the way */
    printf("Move disk %d from %c to %c\n", n, from, to);  /* move the largest disk */
    towerOfHanoi(n - 1, via, from, to);       /* move n-1 disks onto the destination */
}

int main()
{
    int n;
    printf("Enter number of disks: ");
    scanf("%d", &n);
    towerOfHanoi(n, 'A', 'B', 'C');
    return 0;
}
```

**Sample run (n=2):**
```
Move disk 1 from A to B
Move disk 2 from A to C
Move disk 1 from B to C
```

## 10.5 Key Takeaways

1. Every recursive function needs a **base case** (to stop) and a **recursive case** (that makes measurable progress toward it).
2. Recursive calls use the **call stack**; too many nested calls without hitting a base case leads to a **stack overflow**.
3. Recursion "unwinds" in reverse order — the last call to reach its base case is the first to return a concrete value, and results propagate back up through each pending call.
4. Naive recursive solutions (like plain Fibonacci) can be inefficient due to repeated recomputation — always weigh elegance against performance.
5. Classic problems like Factorial, Fibonacci, GCD, and Tower of Hanoi are the standard vehicles for practicing and understanding recursive thinking.
