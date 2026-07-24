# Chapter 17 — Lecture: Structures

## 📖 Reference: *Let Us C*, 19th Edition — Chapter 17: "Structures"

---

## 17.1 Why Use Structures?

Arrays group multiple values of the **same** type. Real-world entities (a student, a book, a point in 2-D space) often need to group **different types** of related data together as one logical unit — this is exactly what a **structure** (`struct`) provides.

```c
struct Student
{
    char name[30];
    int rollNumber;
    float cgpa;
};
```

This declares a new **type**, `struct Student`, but does **not** yet create any actual variable. To create (and use) a structure variable:

```c
struct Student s1;

strcpy(s1.name, "Ayan");
s1.rollNumber = 101;
s1.cgpa = 8.75;

printf("%s, Roll: %d, CGPA: %.2f\n", s1.name, s1.rollNumber, s1.cgpa);
```

- The **dot operator (`.`)** accesses a member of a structure variable.
- You can also declare and initialize in one step: `struct Student s1 = {"Ayan", 101, 8.75};`.

### `typedef` for Convenience

```c
typedef struct
{
    char name[30];
    int rollNumber;
    float cgpa;
} Student;

Student s1;   /* no need to repeat 'struct' every time */
```

## 17.2 Intricacies of Structures

### 17.2.1 Structure Declaration

```c
struct StructName
{
    type1 member1;
    type2 member2;
    ...
};   /* semicolon is MANDATORY here */
```

### 17.2.2 Storage of Structure Elements

Structure members are generally stored in **contiguous memory**, in the order declared — but the compiler may insert **padding bytes** between members for **alignment** purposes (so each member starts at an address matching its natural alignment requirement, for CPU efficiency). This means `sizeof(struct Student)` can be **larger** than the simple sum of its members' individual sizes.

```c
struct Example
{
    char c;    /* 1 byte */
    int i;     /* 4 bytes, but likely needs to start at a 4-byte-aligned address */
};
/* sizeof(struct Example) might be 8, not 5, due to 3 padding bytes after 'c' */
```

### 17.2.3 Copying of Structure Elements

Unlike arrays, an **entire structure variable can be copied with a simple `=` assignment** — this copies every member's value at once:

```c
struct Student s1 = {"Ayan", 101, 8.75};
struct Student s2;

s2 = s1;   /* legal! copies ALL members of s1 into s2 in one statement */
```

### 17.2.4 Nested Structures

A structure can contain another structure as a member:

```c
struct Date
{
    int day, month, year;
};

struct Student
{
    char name[30];
    struct Date dob;   /* nested structure member */
};

struct Student s1;
s1.dob.day = 15;    /* chained dot operators to reach the nested member */
s1.dob.month = 8;
s1.dob.year = 2007;
```

### 17.2.5 Passing Structure Elements / Structure Variables to Functions

Individual members can be passed like ordinary variables of their type. **Entire structures**, when passed by value, are **copied wholesale** into the function's parameter — modifications inside the function do **not** affect the caller's original structure (just like ordinary call by value):

```c
void display(struct Student s)   /* receives a COPY of the whole structure */
{
    printf("%s, %d, %.2f\n", s.name, s.rollNumber, s.cgpa);
}
```

To modify the caller's original structure, pass a **pointer to the structure** instead (call by reference, exactly as with ordinary variables):

```c
void increaseCgpa(struct Student *s, float bonus)
{
    s->cgpa += bonus;   /* '->' is used to access members THROUGH a pointer */
}
```

### The Arrow Operator `->`

When you have a **pointer to a structure**, use `->` (instead of `.`) to access its members — `ptr->member` is exactly equivalent to `(*ptr).member`:

```c
struct Student s1 = {"Ayan", 101, 8.75};
struct Student *ptr = &s1;

printf("%s\n", ptr->name);       /* preferred, idiomatic style */
printf("%s\n", (*ptr).name);     /* equivalent, but less common in practice */
```

### 17.2.6 Packing Structure Elements — `#pragma pack`

Compiler-specific directives (e.g., `#pragma pack(1)`) can force the compiler to **remove padding**, making structures more compact — at the cost of potentially slower unaligned memory access on some architectures. This is an advanced, compiler-dependent technique typically used in low-level or embedded programming, not needed for everyday application code.

## 17.3 Array of Structures

Just as you'd use an array of `int`s to store many numbers, an **array of structures** stores many records of the same "shape":

```c
#include <stdio.h>

struct Student
{
    char name[30];
    int rollNumber;
    float cgpa;
};

int main()
{
    struct Student students[3];
    int i;

    for (i = 0; i < 3; i++)
    {
        printf("Enter name, roll number, CGPA for student %d: ", i + 1);
        scanf("%s %d %f", students[i].name, &students[i].rollNumber, &students[i].cgpa);
    }

    printf("\n--- Student Records ---\n");
    for (i = 0; i < 3; i++)
        printf("%-20s %5d %6.2f\n", students[i].name, students[i].rollNumber, students[i].cgpa);

    return 0;
}
```

## 17.4 Uses of Structures

- Representing real-world composite entities: students, employees, books, points, dates, complex numbers.
- Building the foundation for more advanced data structures (linked lists, trees, graphs) — each "node" is typically a structure containing data plus one or more pointers to other nodes.
- Grouping related function parameters into one logical unit, simplifying function signatures.

## 17.5 Worked Programs

### Program 1: Structure for a 2-D Point, with a Distance Function

```c
#include <stdio.h>
#include <math.h>

struct Point
{
    float x, y;
};

float distance(struct Point p1, struct Point p2)
{
    return sqrt(pow(p2.x - p1.x, 2) + pow(p2.y - p1.y, 2));
}

int main()
{
    struct Point a = {0, 0};
    struct Point b = {3, 4};

    printf("Distance = %.2f\n", distance(a, b));
    return 0;
}
```

**Output:** `Distance = 5.00`

### Program 2: Array of Structures — Finding the Topper

```c
#include <stdio.h>

struct Student
{
    char name[30];
    float cgpa;
};

int main()
{
    struct Student students[3] = {
        {"Alice", 8.5},
        {"Bob", 9.2},
        {"Charlie", 7.8}
    };
    int i, topperIndex = 0;

    for (i = 1; i < 3; i++)
    {
        if (students[i].cgpa > students[topperIndex].cgpa)
            topperIndex = i;
    }

    printf("Topper: %s with CGPA %.2f\n", students[topperIndex].name, students[topperIndex].cgpa);
    return 0;
}
```

### Program 3: Modifying a Structure via a Pointer

```c
#include <stdio.h>

struct Account
{
    char holderName[30];
    float balance;
};

void deposit(struct Account *acc, float amount)
{
    acc->balance += amount;   /* modifies the CALLER's structure, thanks to the pointer */
}

int main()
{
    struct Account myAccount = {"Ayan", 1000.0};

    deposit(&myAccount, 500.0);

    printf("%s's balance: Rs %.2f\n", myAccount.holderName, myAccount.balance);
    return 0;
}
```

**Output:** `Ayan's balance: Rs 1500.00`

## 17.6 Key Takeaways

1. `struct` groups related members of possibly *different* types into one logical, named unit.
2. `.` accesses a member of a structure variable directly; `->` accesses a member through a structure pointer.
3. Unlike arrays, entire structures **can** be copied with a plain `=` assignment.
4. Passing a structure by value copies the whole thing; pass a pointer (`struct Type *`) to let a function modify the caller's original structure.
5. Padding/alignment can make `sizeof(struct)` larger than the sum of its members' sizes — this is a compiler/platform detail, not a bug.
