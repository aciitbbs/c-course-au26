# Chapter 10 — Quiz: Recursion

## 📖 Topics Covered: Base case, recursive case, call stack, tracing recursion, classic recursive problems

---

## Part A: Multiple Choice Questions (5)

### Q1. What are the two essential parts every correctly written recursive function must have?

A) A loop and a condition
B) A base case and a recursive case
C) A pointer and a return value
D) An array and an index

<details>
<summary><b>Answer</b></summary>

**B) A base case and a recursive case**

The base case stops the recursion at a simple, directly answerable situation; the recursive case calls the function again on a smaller/simpler version of the problem, making progress toward the base case.
</details>

---

### Q2. What happens if a recursive function is written without a reachable base case?

A) It runs once and returns 0
B) The compiler rejects it with an error
C) It calls itself indefinitely, eventually causing a stack overflow
D) It automatically converts itself into a loop

<details>
<summary><b>Answer</b></summary>

**C) It calls itself indefinitely, eventually causing a stack overflow**

Without ever reaching a base case, the function keeps calling itself, consuming more and more stack memory for each nested call, until the program crashes with a stack overflow — the recursive analogue of an infinite loop.
</details>

---

### Q3. What is the value returned by `factorial(3)` using `long factorial(int n) { if (n==0) return 1; return n*factorial(n-1); }`?

A) `3`
B) `6`
C) `9`
D) `1`

<details>
<summary><b>Answer</b></summary>

**B) `6`**

`factorial(3) = 3 * factorial(2) = 3 * (2 * factorial(1)) = 3 * 2 * (1 * factorial(0)) = 3 * 2 * 1 * 1 = 6`.
</details>

---

### Q4. In the recursive Fibonacci function `fibonacci(n) = fibonacci(n-1) + fibonacci(n-2)` (with base cases at 0 and 1), why is this naive version considered inefficient for large `n`?

A) It uses too much memory storing an array
B) It recomputes the same sub-values many times, leading to exponential time complexity
C) It does not have a valid base case
D) C does not allow functions to call themselves twice

<details>
<summary><b>Answer</b></summary>

**B) It recomputes the same sub-values many times, leading to exponential time complexity**

Each call to `fibonacci(n)` spawns two more calls, `fibonacci(n-1)` and `fibonacci(n-2)`, and their sub-calls overlap heavily — e.g., `fibonacci(5)` and `fibonacci(4)` both eventually call `fibonacci(3)` independently — causing a huge amount of duplicated work as `n` grows.
</details>

---

### Q5. In the Tower of Hanoi recursive solution `towerOfHanoi(n-1, from, to, via); move disk n; towerOfHanoi(n-1, via, from, to);`, what is the base case?

A) `n == 1`
B) `n == 0`
C) There is no base case; it relies purely on the print statement
D) `from == to`

<details>
<summary><b>Answer</b></summary>

**B) `n == 0`**

The function immediately returns (does nothing) when `n == 0`, since there are no disks left to move — this is the condition that stops the recursive descent.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain, in your own words, what the "call stack" is and how it relates to recursion. Use `factorial(3)` as your example.

<details>
<summary><b>Model Answer</b></summary>

The **call stack** is the region of memory where the system keeps track of active (not-yet-completed) function calls, each represented by its own "stack frame" containing that call's local variables and parameters. When a function calls itself recursively, a **new stack frame is pushed** for each call, layering on top of the previous ones, until a base case is reached and calls begin to **return (pop off the stack)** in reverse order.

For `factorial(3)`: calling `factorial(3)` pushes a frame that calls `factorial(2)`, which pushes another frame calling `factorial(1)`, which pushes a frame calling `factorial(0)`. At `factorial(0)`, the base case returns `1` immediately (no further calls), and then each pending multiplication resolves as the stack unwinds: `factorial(1)` returns `1*1=1`, `factorial(2)` returns `2*1=2`, and finally `factorial(3)` returns `3*2=6`.
</details>

---

### Q2. Compare recursion and iteration as two approaches to solving the same problem. When might you prefer one over the other?

<details>
<summary><b>Model Answer</b></summary>

Both recursion and iteration (loops) can solve the same class of repetitive problems — anything expressible recursively can, in principle, also be written iteratively, and vice versa.

- **Recursion** tends to produce more elegant, naturally readable code for problems that are *inherently* self-similar/recursive in structure, such as Tower of Hanoi, tree traversal, or divide-and-conquer algorithms — the recursive formulation closely mirrors the mathematical definition of the problem.
- **Iteration** is usually more memory-efficient (constant stack usage, no per-call overhead) and can be faster for simple counting/accumulation tasks, since it avoids the overhead of repeated function calls and stack frame management.

In practice: prefer recursion when it makes the solution dramatically clearer and the expected recursion depth is small/bounded; prefer iteration when performance/memory matters, or when the recursive depth could be large enough to risk a stack overflow.
</details>

---

### Q3. Trace `sumOfN(4)` step by step, using `int sumOfN(int n) { if (n==0) return 0; return n + sumOfN(n-1); }`, showing both the "descending" calls and the "returning" values.

<details>
<summary><b>Model Answer</b></summary>

**Descending (calls being made):**
```
sumOfN(4) calls sumOfN(3)
sumOfN(3) calls sumOfN(2)
sumOfN(2) calls sumOfN(1)
sumOfN(1) calls sumOfN(0)
sumOfN(0) -> base case, returns 0 immediately
```

**Returning (unwinding, resolving the pending additions):**
```
sumOfN(1) = 1 + sumOfN(0) = 1 + 0 = 1
sumOfN(2) = 2 + sumOfN(1) = 2 + 1 = 3
sumOfN(3) = 3 + sumOfN(2) = 3 + 3 = 6
sumOfN(4) = 4 + sumOfN(3) = 4 + 6 = 10
```

**Final result:** `sumOfN(4) = 10`.
</details>

---

### Q4. Explain why the naive recursive GCD function `gcd(a, b) = (b == 0) ? a : gcd(b, a % b);` terminates for any pair of non-negative integers. What guarantees it eventually reaches its base case?

<details>
<summary><b>Model Answer</b></summary>

The base case is `b == 0`, at which point the function immediately returns `a` without further recursion. The recursive step replaces `(a, b)` with `(b, a % b)`. Since `a % b` is always **strictly smaller than `b`** (by the definition of the modulus operation, as long as `b > 0`), the second argument keeps strictly shrinking with every recursive call. A strictly decreasing sequence of non-negative integers cannot decrease forever — it must eventually hit `0`, at which point the base case triggers and the recursion terminates. This guarantee (a value that must strictly decrease toward a fixed stopping point) is exactly what any well-formed recursive function needs to ensure it always terminates.
</details>

---

### Q5. Explain, conceptually, the recursive strategy behind the Tower of Hanoi solution for moving `n` disks from peg `from` to peg `to`, using peg `via` as the auxiliary. Why does the solution call itself twice?

<details>
<summary><b>Model Answer</b></summary>

The recursive insight is: to move `n` disks from `from` to `to` (using `via` as a spare peg), you can:

1. First move the top `n-1` (smaller) disks from `from` to the *auxiliary* peg `via` (temporarily out of the way), using `to` as the spare during this sub-move.
2. Then move the single largest (`n`th) disk directly from `from` to `to` (this is now legal since only the largest disk remains on `from`).
3. Finally, move the `n-1` disks that are now sitting on `via` onto `to` (on top of the largest disk), using `from` as the spare during this sub-move.

Both step 1 and step 3 are themselves smaller instances of the exact same "move some disks from one peg to another using a third as auxiliary" problem, just with `n-1` disks and possibly different peg roles — which is why the function calls itself **twice**, once before and once after directly moving the largest disk. The base case (`n == 0`, no disks to move) ensures the recursion eventually stops.
</details>
