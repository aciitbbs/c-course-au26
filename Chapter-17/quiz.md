# Chapter 17 — Quiz: Structures

## 📖 Topics Covered: `struct` declaration, dot vs arrow operator, structure copying, passing structures to functions, array of structures

---

## Part A: Multiple Choice Questions (5)

### Q1. What is the correct way to access the `rollNumber` member of a structure variable `s1` of type `struct Student`?

A) `s1->rollNumber`
B) `s1.rollNumber`
C) `s1[rollNumber]`
D) `rollNumber(s1)`

<details>
<summary><b>Answer</b></summary>

**B) `s1.rollNumber`**

The dot operator (`.`) accesses a member directly from a structure *variable*. The arrow operator (`->`) is used instead when you have a *pointer* to the structure.
</details>

---

### Q2. Given `struct Student *ptr = &s1;`, which of these correctly accesses `s1`'s `cgpa` member through `ptr`?

A) `ptr.cgpa`
B) `*ptr.cgpa`
C) `ptr->cgpa`
D) `ptr[cgpa]`

<details>
<summary><b>Answer</b></summary>

**C) `ptr->cgpa`**

`ptr->cgpa` is the idiomatic way to access a member through a pointer, exactly equivalent to `(*ptr).cgpa`.
</details>

---

### Q3. Is `struct Student s2 = s1;` (copying one whole structure variable into another) legal in C?

A) No, this is always a compilation error
B) Yes — it copies every member of `s1` into `s2` in a single statement
C) Only legal if the structure has no `char` array members
D) Only legal using `memcpy`, never plain assignment

<details>
<summary><b>Answer</b></summary>

**B) Yes — it copies every member of `s1` into `s2` in a single statement**

Unlike arrays, entire structure *variables* can be directly assigned with `=`, which copies every member's value from the source into the destination in one operation.
</details>

---

### Q4. If a function receives a structure parameter by value, `void show(struct Student s)`, and modifies `s.cgpa` inside the function, what happens to the caller's original structure?

A) The caller's structure is also modified
B) The caller's structure remains unchanged, since `s` is a copy
C) A compilation error occurs
D) Only `cgpa` is shared between caller and callee, all other members are copied

<details>
<summary><b>Answer</b></summary>

**B) The caller's structure remains unchanged, since `s` is a copy**

Passing a structure by value copies the entire structure into the function's local parameter, exactly like passing an ordinary scalar variable by value. Any changes inside the function affect only that local copy.
</details>

---

### Q5. Why might `sizeof(struct Example)` be larger than the simple sum of the sizes of its individual members?

A) The compiler always doubles structure sizes for safety
B) Padding bytes may be inserted between members for memory alignment purposes
C) `struct` always reserves extra space for a hidden member
D) This can never happen; `sizeof` always equals the exact sum of member sizes

<details>
<summary><b>Answer</b></summary>

**B) Padding bytes may be inserted between members for memory alignment purposes**

Compilers often insert unused padding bytes between (or after) members so that each member starts at a memory address matching its natural alignment requirement, which can make the CPU access those members more efficiently — at the cost of the structure's total size being larger than the raw sum of its members.
</details>

---

## Part B: Short Descriptive Questions (5)

### Q1. Explain the difference between `struct Student s1;` and `struct Student *ptr;`. What does each variable actually store, and which operator (`.` or `->`) is used with each?

<details>
<summary><b>Model Answer</b></summary>

`struct Student s1;` declares `s1` as an **actual structure variable** — it directly holds all of the structure's member data in its own memory. Members are accessed using the **dot operator**: `s1.rollNumber`.

`struct Student *ptr;` declares `ptr` as a **pointer to a structure** — it does not hold the structure's data itself, only the *address* of a `struct Student` variable somewhere else in memory (e.g., after `ptr = &s1;`). Members are accessed **through** the pointer using the **arrow operator**: `ptr->rollNumber`, which is exactly equivalent to writing `(*ptr).rollNumber`.
</details>

---

### Q2. Why must you pass a pointer to a structure (rather than the structure by value) if a function needs to modify the caller's original structure? Illustrate with a short example.

<details>
<summary><b>Model Answer</b></summary>

Passing a structure **by value** copies the *entire* structure's contents into the function's local parameter — exactly the same "call by value" principle that applies to ordinary scalar variables. Any changes made to that local copy inside the function are completely invisible to the caller once the function returns, since the caller's original structure was never touched.

To let a function genuinely modify the caller's structure, you must pass its **address** (a pointer), and access/modify members through that pointer using `->`:

```c
void raiseSalary(struct Employee *emp, float amount)
{
    emp->salary += amount;   /* modifies the ORIGINAL structure via its address */
}

/* Called as: raiseSalary(&myEmployee, 5000); */
```

This mirrors exactly the same call-by-reference principle studied for ordinary variables in Chapter 9 — structures are no exception to that rule.
</details>

---

### Q3. What is a nested structure? Give an example declaration and show how to access a doubly-nested member.

<details>
<summary><b>Model Answer</b></summary>

A **nested structure** occurs when one structure type is used as a *member* inside another structure — allowing complex, hierarchical data to be modeled naturally.

```c
struct Date
{
    int day, month, year;
};

struct Student
{
    char name[30];
    struct Date dob;   /* a structure nested inside another structure */
};

struct Student s1;
s1.dob.year = 2007;    /* chained dot operators: first reach 'dob', then its 'year' member */
```

To access a member of the nested structure, you simply chain dot operators: first navigate to the outer structure's member that *is* the nested structure (`s1.dob`), then apply another `.` to reach the desired inner member (`.year`).
</details>

---

### Q4. Explain why an array of structures is a natural way to represent a list of records (e.g., multiple students), and write a short loop that finds the student with the highest CGPA in such an array.

<details>
<summary><b>Model Answer</b></summary>

A `struct Student` bundles all the fields relevant to *one* student into a single unit. Since real applications typically need to manage *many* students at once, and each one has the exact same "shape" (the same set of fields), an **array of structures** (`struct Student students[N];`) is the natural extension — exactly analogous to how an array of `int` stores many numbers of the same type, here we store many *records* of the same composite type.

```c
int topperIndex = 0, i;
for (i = 1; i < n; i++)
{
    if (students[i].cgpa > students[topperIndex].cgpa)
        topperIndex = i;
}
/* students[topperIndex] now refers to the record with the highest CGPA */
```

This loop simply generalizes the familiar "find the maximum" pattern (Chapter 13) to compare one specific *member* (`cgpa`) across every element of the structure array, tracking the index of the best one seen so far.
</details>

---

### Q5. What is structure padding, and why does the compiler introduce it rather than always packing members tightly together?

<details>
<summary><b>Model Answer</b></summary>

**Structure padding** refers to extra, unused bytes that the compiler may silently insert between (or after) a structure's members, so that each member begins at a memory address satisfying its own natural **alignment requirement** (e.g., a 4-byte `int` is typically most efficient when it starts at an address that is a multiple of 4).

The compiler introduces this padding because most CPU architectures can read/write properly-aligned data **faster** (in a single memory-access operation), whereas misaligned access can require multiple slower operations, or on some architectures, even trigger a hardware fault. The trade-off is that the structure's total `sizeof` may end up **larger** than the naive sum of its individual members' sizes — a deliberate compiler decision favouring runtime access speed over minimal memory footprint. (For memory-critical applications, compiler-specific directives like `#pragma pack` can override this default behaviour, at the cost of potentially slower member access.)
</details>
