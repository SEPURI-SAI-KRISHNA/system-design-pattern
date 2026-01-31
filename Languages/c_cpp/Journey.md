# Journey

C and C++ are "Native" languages. They follow the same path: Source Code $\rightarrow$ Machine Code (Binary) $\rightarrow$ CPU. There is no "Virtual Machine" or "Interpreter" in the middle. The Operating System runs them directly.

This flow applies to C and C++. The only major difference is that C++ has a more complex compiler ("name mangling" and classes), but the phases are identical.

Let's assume you have a file sum.c

```
#include <stdio.h>

int main() {
    int x = 5;
    int y = 10;
    int result = x + y;
    printf("%d\n", result);
    return 0;
}
```

Below is the code Journey.

---

## Phase 1
### Pre-processing (Text Replacement)
Before "compilation" actually starts, a tool called the Preprocessor runs.

1. __Header Expansion__: It sees #include <stdio.h>. It finds that file on your hard drive (containing function declarations for input/output) and literally copies and pastes the entire content of that file into your sum.c.

2. __Cleanup__: It strips out all your comments (// comment).

3. __Macro Replacement__: If you had #define PI 3.14, it replaces every "PI" with "3.14".

- __Result__: A massive pure C text file (often called a "Translation Unit").

---


## Phase 2

### Compilation (C to Assembly)
The compiler (like GCC or Clang) translates that pure C code into Assembly Language. Assembly is human-readable machine instructions specific to your CPU (e.g., x86 or ARM).

- The line int x = 5; might become the assembly instruction: movl $5, -4(%rbp) (Move the value 5 into the memory address for variable x).

- Result: An assembly file (sum.s).

---


## Phase 3

### Assembly (Assembly to Machine Code)

The Assembler takes the assembly code and converts it into raw binary (0s and 1s).

- movl $5... becomes something like c7 45 fc 05 00 00 00.

- This produces an Object File (sum.o on Linux or sum.obj on Windows).

- Crucial Note: This file is not yet executable. It contains your code, but it doesn't know where printf is defined (since printf lives in the system library, not your file).

---


## Phase 4

### Linking (The Final Assembly)
The Linker resolves the missing pieces.

1. It looks at your object file and sees a placeholder: "Call function printf (address unknown)."

2. It finds the Standard C Library (libc) on your system.

3. It merges your code with the necessary startup code (crt0) and the library references.

4. It produces the final Executable Binary (a.out or sum.exe).

---


## Phase 5

### The OS Loader (Run Time)
You type ./sum.exe. The OS takes over.

1. __Loading__: The OS "Loader" reads the binary from the disk.

2. __Memory Mapping__: It requests RAM and maps the code segment (text), data segment (globals), and sets up the Stack.

3. __Entry Point__: The OS jumps the CPU to the main() function's memory address.

---


## Phase 6

### CPU Execution (Hardware Level)
Unlike Python or Java, there is no "Virtual Machine" managing this. The CPU executes instructions directly from RAM.

1. __Fetch__: CPU reads the binary instruction for movl $5, ....

2. __Decode__: CPU understands this means "Move data."

3. __Execute__: CPU writes 5 directly to the stack memory address.

4. __Add__: CPU loads 5 and 10 into hardware registers (e.g., EAX, EBX), triggers the ALU (Arithmetic Logic Unit) to add them, and stores 15.

5. __Syscall__: When printf runs, it eventually triggers a specific CPU interrupt (syscall) that hands control back to the OS kernel to "put these characters on the screen."


