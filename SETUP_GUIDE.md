# Setting Up C Programming Environment

This guide will help you install a C compiler and write your first program on **Windows**, **macOS**, or **Linux**.

---

## Table of Contents

1. [Windows Setup](#1-windows-setup)
2. [macOS Setup](#2-macos-setup)
3. [Linux Setup](#3-linux-setup)
4. [Recommended Code Editor (VS Code)](#4-recommended-code-editor-vs-code)
5. [Writing Your First Program](#5-writing-your-first-program)
6. [Compiling and Running](#6-compiling-and-running)
7. [Common Errors & Troubleshooting](#7-common-errors--troubleshooting)

---

## 1. Windows Setup

### Option A: MinGW-w64 (Recommended)

**Step 1:** Download the MinGW-w64 installer from [https://www.mingw-w64.org/](https://www.mingw-w64.org/) or use the MSYS2 method below.

**Step 2: Using MSYS2 (Easier Method)**

1. Download MSYS2 from [https://www.msys2.org/](https://www.msys2.org/)
2. Run the installer and follow the defaults
3. Open the **MSYS2 UCRT64** terminal
4. Run the following command to install GCC:

```bash
pacman -S mingw-w64-ucrt-x86_64-gcc
```

5. Verify installation:

```bash
gcc --version
```

**Step 3: Add to System PATH**

1. Open **Settings** → **System** → **About** → **Advanced system settings**
2. Click **Environment Variables**
3. Under **System Variables**, find `Path` and click **Edit**
4. Add the path: `C:\msys64\ucrt64\bin` (adjust if installed elsewhere)
5. Click **OK** on all dialogs
6. Open a **new** Command Prompt and verify:

```cmd
gcc --version
```

You should see output like:
```
gcc (Rev2, Built by MSYS2 project) 13.x.x
```

### Option B: Using WSL (Windows Subsystem for Linux)

If you prefer a Linux-like experience on Windows:

1. Open PowerShell as Administrator and run:
```powershell
wsl --install
```
2. Restart your computer
3. Open Ubuntu from the Start Menu
4. Follow the [Linux Setup](#3-linux-setup) instructions below

---

## 2. macOS Setup

### Using Xcode Command Line Tools (Recommended)

**Step 1:** Open the **Terminal** app (Applications → Utilities → Terminal)

**Step 2:** Install the command line tools:

```bash
xcode-select --install
```

A dialog will appear — click **Install** and wait for the download to complete.

**Step 3:** Verify installation:

```bash
gcc --version
```

You should see output mentioning `Apple clang` (Apple ships Clang as `gcc` — it works identically for this course).

### Alternative: Install GCC via Homebrew

If you want the actual GCC compiler:

1. Install Homebrew (if not already installed):
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. Install GCC:
```bash
brew install gcc
```

3. Use `gcc-13` (or the version number shown) instead of `gcc`:
```bash
gcc-13 --version
```

---

## 3. Linux Setup

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install build-essential
gcc --version
```

### Fedora / CentOS / RHEL

```bash
sudo dnf install gcc
gcc --version
```

### Arch Linux

```bash
sudo pacman -S gcc
gcc --version
```

---

## 4. Recommended Code Editor (VS Code)

We recommend **Visual Studio Code** as your code editor.

### Installation

1. Download VS Code from [https://code.visualstudio.com/](https://code.visualstudio.com/)
2. Install and launch it

### Essential Extensions

Open VS Code, go to **Extensions** (Ctrl+Shift+X / Cmd+Shift+X) and install:

1. **C/C++** by Microsoft — IntelliSense, debugging, and code browsing
2. **Code Runner** — Run code with a single click

### Configure Code Runner (Optional)

1. Open **Settings** (Ctrl+, / Cmd+,)
2. Search for `code-runner.executorMap`
3. Click **Edit in settings.json**
4. Add or modify the C entry:
```json
"code-runner.executorMap": {
    "c": "cd $dir && gcc -Wall -Wextra $fileName -o $fileNameWithoutExt -lm && $dir$fileNameWithoutExt"
}
```

### Integrated Development Environments (IDEs)

If you prefer an all-in-one software that includes a code editor, compiler, and interactive debugger out of the box (an IDE), here are the best options for students:

1. **CLion by JetBrains (Windows, macOS, Linux)**
   - **Why use it:** Extremely powerful, modern, and interactive. Provides excellent debugging, code completion, and error detection.
   - **Cost:** Free for university students (sign up with your `.edu` email).
   - **Link:** [https://www.jetbrains.com/clion/](https://www.jetbrains.com/clion/)

2. **Code::Blocks (Windows mainly)**
   - **Why use it:** Very easy to install on Windows because it comes with the MinGW compiler bundled. You can install it and immediately start running C code.
   - **Tip:** When downloading for Windows, make sure to get the `mingw-setup.exe` version.
   - **Link:** [https://www.codeblocks.org/](https://www.codeblocks.org/)

3. **Xcode (macOS only)**
   - **Why use it:** The official IDE from Apple. Excellent performance and integration, though it is a very large download.
   - **Link:** Download directly from the Mac App Store.

4. **Replit (Web-based)**
   - **Why use it:** No installation required! Write, compile, and interactively test your C code directly in your web browser. Great for quick testing.
   - **Link:** [https://replit.com/](https://replit.com/)

---

## 5. Writing Your First Program

Create a file called `hello.c` with the following content:

```c
#include <stdio.h>

int main() {
    printf("Hello, IIT Bhubaneswar!\n");
    printf("Welcome to Programming in C\n");
    return 0;
}
```

Save the file in a folder you can easily navigate to (e.g., `Desktop/c-programs/`).

---

## 6. Compiling and Running

### Using Terminal / Command Prompt

**Step 1:** Navigate to the folder containing your `.c` file:

```bash
cd Desktop/c-programs
```

**Step 2:** Compile the program:

```bash
gcc hello.c -o hello
```

This creates an executable file named `hello` (or `hello.exe` on Windows).

**Step 3:** Run the executable:

- **macOS / Linux:**
```bash
./hello
```

- **Windows (Command Prompt):**
```cmd
hello.exe
```

**Expected Output:**
```
Hello, IIT Bhubaneswar!
Welcome to Programming in C
```

### Compilation Flags (Recommended)

Always compile with warning flags to catch potential issues:

```bash
gcc -Wall -Wextra -o hello hello.c
```

| Flag | Purpose |
|------|---------|
| `-Wall` | Enable all common warnings |
| `-Wextra` | Enable extra warning checks |
| `-o <name>` | Specify output executable name |
| `-lm` | Link the math library (needed for `math.h` functions like `sqrt`, `pow`) |
| `-std=c99` | Use the C99 standard (allows declaring variables inside `for` loops) |

### Quick Compile & Run (One Command)

```bash
gcc -Wall -Wextra hello.c -o hello && ./hello
```

On Windows:
```cmd
gcc -Wall -Wextra hello.c -o hello.exe && hello.exe
```

---

## 7. Common Errors & Troubleshooting

### "gcc is not recognized" (Windows)

**Cause:** GCC is not added to the system PATH.

**Fix:** Follow Step 3 in the Windows Setup section above. Make sure to open a **new** terminal after modifying PATH.

### "permission denied" (macOS/Linux)

**Cause:** The compiled file doesn't have execute permission.

**Fix:**
```bash
chmod +x hello
./hello
```

### "undefined reference to sqrt" or math functions

**Cause:** The math library is not linked.

**Fix:** Add `-lm` flag:
```bash
gcc program.c -o program -lm
```

### "expected ';' before '}'"

**Cause:** Missing semicolon at the end of a statement.

**Fix:** Check that every statement ends with `;`

### "implicit declaration of function"

**Cause:** Missing `#include` for the required header file.

**Fix:** Add the appropriate header:
- `printf` / `scanf` → `#include <stdio.h>`
- `sqrt` / `pow` → `#include <math.h>`
- `strlen` / `strcpy` → `#include <string.h>`
- `malloc` / `free` → `#include <stdlib.h>`

### Program runs but shows garbage values

**Cause:** Using uninitialized variables.

**Fix:** Always initialize variables before use:
```c
int count = 0;  // ✅ Good
int count;      // ❌ May contain garbage
```

---

## Quick Reference Card

| Task | Command |
|------|---------|
| Compile | `gcc program.c -o program` |
| Compile with warnings | `gcc -Wall -Wextra program.c -o program` |
| Compile with math library | `gcc program.c -o program -lm` |
| Run (macOS/Linux) | `./program` |
| Run (Windows) | `program.exe` |
| Check GCC version | `gcc --version` |

---

*Happy coding! 🚀 If you face any issues, reach out to your TA or visit office hours.*
