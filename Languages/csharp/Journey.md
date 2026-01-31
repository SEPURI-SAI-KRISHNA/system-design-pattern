# Journey

C# is designed for safety and portability, so it uses a Virtual Machine model similar to Java.

C# sits in a "sweet spot" between the compilation of C and the virtual machine of Java. Its defining characteristic is that it compiles to an "Intermediate Language" first, and then compiles again into machine code right when you run it.

Let's assume you have a file named Sum.cs

```
using System;

class Sum {
    static void Main() {
        int x = 5;
        int y = 10;
        int result = x + y;
        Console.WriteLine(result);
    }
}
```

Below is the code Journey.

---

## Phase 1

### Compile Time (csc Sum.cs)

You run the C# compiler (Roslyn). Unlike C or C++, this does not produce code your CPU can read.

1. __Lexical Analysis & Parsing__

    - The compiler reads Sum.cs.

    - Lexer: Breaks source into tokens (class, Sum, {, int).

    - Parser: Builds the Abstract Syntax Tree (AST) and checks for syntax errors (like missing semicolons).

2. __IL Generation (The "Half-way" Compilation)__

    - The compiler translates the AST into CIL (Common Intermediate Language) (often just called IL or MSIL).

    - Metadata: It also generates a table describing your classes, methods, and types.

    - Packaging: It packages the IL and Metadata into a Managed Assembly (e.g., Sum.exe or Sum.dll).

        - Crucial Detail: Even though it ends in .exe, it is not binary machine code. It contains IL instructions that look like this:

            - ldc.i4.5 (Load constant integer 5)

            - stloc.0 (Store in local variable 0)


---


## Phase 2

### Runtime Initialization (The CLR)
You click run or type Sum.exe.

3. __Bootstrapping__

    - The Operating System (Windows/Linux) sees the file header.

    - It realizes this is a .NET assembly, not a standard native app.

    - It loads the CLR (Common Language Runtime) into memory. The CLR is the "Virtual Engine" of .NET.

4. __CLR Startup__ The CLR initializes critical services:

    - Garbage Collector (GC): Sets up memory management.

    - Class Loader: Reads the Metadata from your .exe to understand the Sum class.

    - Verifier: Checks the IL code to ensure type safety (e.g., ensuring you aren't trying to access private memory).

---


## Phase 3

### JIT Compilation (Just-In-Time)

This is where C# performs its magic. The CLR does not interpret the IL (like Python does). It compiles it again.

5. __The JIT Trigger__

    - The CLR prepares to run the Main() method.

    - It sees that Main is currently just IL code, not native CPU code.

    - It calls the JIT (Just-In-Time) Compiler.

6. __Native Code Generation__

    - The JIT reads the IL for Main.

    - It translates ldc.i4.5 into the actual machine code for your specific processor (e.g., MOV EAX, 5 for Intel x64 or MOV R0, #5 for ARM).

    - It stores this native code in a protected memory area.

    - Optimization: The JIT might optimize the code (e.g., inlining small methods) during this step.

---


## Phase 4

### Execution

Now, the CPU executes the freshly minted native code.

7. __Stack Frame Execution__ The steps here look very similar to the C/C++ execution because it is now running native code:

    - Step A (int x = 5): CPU moves 5 into a register or stack location.

    - Step B (int y = 10): CPU moves 10 into a register or stack location.

    - Step C (int result = x + y):

       - CPU loads values into ALU registers.

       - CPU performs the ADD instruction.

       - CPU stores 15 in the location for result.

---


## Phase 5

### Output & Managed Context

8. __Console.WriteLine(result)__

    - This call is handled by the Base Class Library (BCL).

    - The CLR looks up the Console class.

    - If WriteLine hasn't been run yet, the JIT compiles it to native code right then and there.

    - Eventually, it calls an OS function (like WriteFile on Windows) to print "15" to the screen.

9. __Termination__

    - The Main method finishes.

    - The CLR shuts down the Garbage Collector and releases all memory back to the OS.

    - The process ends.

---

### Summary of the "Unique" C# Part

- The defining feature of C# flow is Phase 3 (JIT).

 - C/C++ compiles to native code on the developer's machine (before distribution).

 - C# compiles to native code on the user's machine (instantly, right when the app opens).

 - This allows a C# .exe to be moved from an Intel computer to an ARM computer (provided the .NET Runtime is installed) and run efficiently on both, because the JIT compiles it specifically for the chip it finds at that moment.

