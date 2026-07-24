# Programming in C — IIT Bhubaneswar

> **Autumn Semester 2026 | UG 1st Year**

## 📋 Course Information

| Detail | Info |
|---|---|
| **Institute** | Indian Institute of Technology Bhubaneswar |
| **Departments** | Mathematics & Computing, Mechanical Engineering |
| **Instructor** | Visiting Faculty, Assistant Professor, Department of Computer Science & Engineering |
| **Semester** | Autumn 2026 (July – December) |
| **Class Strength** | ~120 students |
| **Teaching Assistants** | 14 (Lectures) + 15 (Labs) |

## 📚 Textbook

**Let Us C — Authentic Guide to C Programming Language**, 19th Edition — *Yashavant P. Kanetkar*

A copy of the textbook is available in the [`book/`](book/) directory for reference. All chapter numbering in this repository follows the **19th edition's** chapter structure (24 chapters + appendices), which is more granular than older editions.

## ⏰ Class Schedule

| Day | Session | Time | Venue |
|-----|---------|------|-------|
| **Monday** | Lab | 10:30 AM – 12:25 PM | Prog-C (Lab) (Gr-2) (LBC 103 and 104) |
| **Thursday** | Lecture | 10:30 AM – 12:25 PM | U13L |

## 📊 Evaluation Scheme

| Component | Marks | Details |
|-----------|-------|---------|
| **Mid-Semester Exam** | 30 | Written exam, **Sep 16–24, 2026** — covers Chapters 1–9 and 13 (up to Pointers and 1-D Arrays) |
| **End-Semester Exam** | 50 | Comprehensive written exam, **Nov 23 – Dec 2, 2026** — covers Chapters 1–20 |
| **Internal Evaluation** | 20 | Quizzes, Lab assignments, Attendance & Participation |
| **Total** | **100** | |

A student is given attendance marks varying linearly from -5 for 0% attendance to +5 for 100% attendance. Students get credit for attendance > 50% and a debit if it is less.

## 📅 Chapter-Based Course Plan (Autumn 2026 Academic Schedule)

This course now follows a **chapter-based structure** (one folder per book chapter), rather than a week-based one — every chapter's lecture, quiz, lab, and flashcards are self-contained in its own `Chapter-XX/` folder, making it easy to study a topic independently of which week it happens to fall in.

### Pre-Mid-Semester (Jul 24 – Sep 11, 2026)

| Week | Dates (Approx.) | Chapter(s) Covered | Topic |
|------|-----------------|---------------------|-------|
| Week 01 | Jul 24–31 | [Chapter 1](Chapter-01/) | Getting Started — C basics, compilation model, variables |
| Week 02 | Aug 3–7 | [Chapter 2](Chapter-02/) | C Instructions — arithmetic, type conversion, precedence |
| Week 03 | Aug 10–14 | [Chapter 3](Chapter-03/) | Decision Control — `if`, `if-else`, nested `if-else` |
| Week 04 | Aug 17–21 | [Chapter 4](Chapter-04/), [Chapter 5](Chapter-05/) | Logical operators, ternary operator; `while` loops |
| Week 05 | Aug 24–28 | [Chapter 6](Chapter-06/), [Chapter 7](Chapter-07/) | `for`/`do-while`, `break`/`continue`; `switch-case` |
| Week 06 | Aug 31 – Sep 4 | [Chapter 8](Chapter-08/), [Chapter 9](Chapter-09/) | Functions; **Pointers** — address-of, dereference, call by reference |
| Week 07 | Sep 7–11 | [Chapter 13](Chapter-13/) | **Arrays** (1-D) — declaration, traversal, pointers & arrays; Mid-Sem revision |

### Mid-Semester Examination

| | |
|---|---|
| **Dates** | September 16–24, 2026 |
| **Syllabus** | [Mid-Sem Exam Guide](Mid-Sem/exam_guidance.md) — Chapters 1–9 and 13 (through Pointers and 1-D Arrays) |
| **Revision** | [Mid-Sem Revision Summary](Mid-Sem/revision_summary.md) |

### Post-Mid-Semester (Sep 28 – Nov 20, 2026)

| Week | Dates (Approx.) | Chapter(s) Covered | Topic |
|------|-----------------|---------------------|-------|
| Week 08 | Sep 28 – Oct 2 | [Chapter 10](Chapter-10/) | Recursion |
| Week 09 | Oct 5–9 | [Chapter 14](Chapter-14/) | Multidimensional Arrays — 2-D/3-D arrays, matrix operations |
| — | Oct 12–16 | — | *Autumn Break (Puja Holidays)* |
| Week 10 | Oct 19–23 | [Chapter 11](Chapter-11/), [Chapter 12](Chapter-12/) | Data Types Revisited (storage classes); The C Preprocessor |
| Week 11 | Oct 26–30 | [Chapter 15](Chapter-15/), [Chapter 16](Chapter-16/) | Strings; Handling Multiple Strings |
| Week 12 | Nov 2–6 | [Chapter 17](Chapter-17/) | Structures |
| Week 13 | Nov 9–13 | [Chapter 18](Chapter-18/), [Chapter 19](Chapter-19/) | Console I/O; File I/O |
| Week 14 | Nov 16–20 | [Chapter 20](Chapter-20/) | More Issues in I/O (`argc`/`argv`, redirection); Comprehensive Revision |

### End-Semester Examination

| | |
|---|---|
| **Dates** | November 23 – December 2, 2026 |
| **Syllabus** | [End-Sem Exam Guide](End-Sem/exam_guidance.md) — Comprehensive, Chapters 1–20 |
| **Revision** | [End-Sem Revision Summary](End-Sem/revision_summary.md) |

> **Note on scope:** Chapters 21 (Operations on Bits), 22 (Miscellaneous Features), 23 (Interview FAQs), and 24 (The Next Level) from the 19th edition are **not** part of the core syllabus for this course, and are left as optional/self-study reference material for interested students directly from the textbook.

## 🛠️ Getting Started

New to C programming? Start here:

1. **[Setup Guide](SETUP_GUIDE.md)** — Install a C compiler on Windows / macOS / Linux
2. **[Chapter 1 Lecture](Chapter-01/lecture.md)** — Your first C program
3. **[Chapter 1 Lab](Chapter-01/lab.md)** — Hands-on exercises

## 📁 Repository Structure

```
Programming-in-C-Course/
├── README.md                    ← You are here
├── SETUP_GUIDE.md                ← C installation guide (Windows / macOS / Linux)
├── book/
│   ├── let-us-c-authentic-guide-to-c-programming-language-19ed.pdf
│   └── Programming-with-C-Byron-Gottfried.pdf
├── Chapter-01/
│   ├── lecture.md                ← Thorough lecture notes covering the entire chapter
│   ├── quiz.md                   ← Exactly 10 questions (5 MCQ + 5 short descriptive), with full answers
│   ├── lab.md                    ← Exactly 3 questions (2 easy @ 3 marks + 1 medium @ 4 marks), complete solutions
│   └── flashcards.md             ← Expanded quick-revision flashcards
├── Chapter-02/ ... Chapter-20/   ← Same structure for every chapter in the syllabus
├── Mid-Sem/
│   ├── exam_guidance.md          ← Mid-sem syllabus, pattern, and exam tips (Chapters 1-9, 13)
│   └── revision_summary.md       ← Cheat sheet (Chapters 1-9, 13)
└── End-Sem/
    ├── exam_guidance.md          ← End-sem syllabus, pattern, and exam tips (Chapters 1-20)
    └── revision_summary.md       ← Cheat sheet (Chapters 1-20)
```

## 📌 Important Notes for Students

- **Lab submissions** must be completed during the Monday lab session.
- **Quizzes** may be conducted at the start of Thursday lectures — stay prepared!
- Use `gcc -Wall -Wextra` flags when compiling to catch potential issues early.
- Always test your code with multiple inputs, including edge cases.
- Use each chapter's **flashcards.md** for quick, spaced revision leading up to the Mid-Sem and End-Sem exams.

## 📬 Contact

For queries, reach out to your assigned TA or the course instructor during office hours.

Instructor email ID: ayanchatterjee@iitbbs.ac.in

---

*Department of Computer Science & Engineering, IIT Bhubaneswar*
