# Journey

Java typically separates these into two distinct stages: Compilation (creating the class file) and Execution (running the class file).

Let's assume you have a file named Sum.java

```
public class Sum {
    public static void main(String[] args) {
        int x = 5;
        int y = 10;
        int result = x + y;
        System.out.println(result);
    }
}
```

Below is the code Journey.

---

## Phase 1

### Compile Time (javac Sum.java)
Unlike Python, Java is statically typed and compiled. You run the Java Compiler (javac).

1. __Lexical Analysis & Parsing__: The compiler reads your .java file.

    - Lexer: Breaks code into tokens: public, class, Sum, {, int, x, =, 5, ;.

    - Parser: Builds an Abstract Syntax Tree (AST). It checks for syntax errors (e.g., missing semicolons, balanced braces).

2. __Semantic Analysis__: This is strict in Java. The compiler checks the AST logic:

    - Are you trying to assign a String to an int variable?

    - Does the variable x exist before you use it?

    - This step ensures type safety before the code ever runs.

3. __Bytecode Generation__: The compiler translates the AST into Java Bytecode. It writes this to a new file: Sum.class.

     - This bytecode is not for your CPU; it is for the Java Virtual Machine (JVM).

---


## Phase 2

### The Shell & Operating System (java Sum)
Now you run the command java Sum.

4. __Loading the JVM__

    - The Command: The OS parses java Sum.

    - The JRE: The OS launches the Java Runtime Environment (JRE).

    - JVM Startup: The JRE starts the Java Virtual Machine (JVM). This is a virtual computer running inside your RAM.

---


## Phase 3
### Class Loading (Inside the JVM)
The JVM doesn't know about your Sum class yet. It has a subsystem called the ClassLoader.

5. __Loading__ 

    - The Application ClassLoader searches the classpath (current directory) for Sum.class, reads the binary data, and creates a Class object in the Metaspace (a special memory area for class metadata).

6. __Linking (Verification & Preparation)__

    - Verification: The Bytecode Verifier checks the .class file to ensure it's safe (e.g., it doesn't try to access memory it shouldn't, or overflow the stack). This prevents malicious code from crashing the host machine.

    - Preparation: The JVM allocates memory for static variables (if any).

7. __Initialization__

    - The JVM looks for the public static void main(String[] args) method to begin execution.

---

## Phase 4
### The Runtime Data Areas

Before execution starts, the JVM arranges its memory

- __Method Area__: Stores the code for 'Sum.class'.

- __Heap__: Where objects are stored (not heavily used in this simple script).

- __Java Stack__: This is where the action happens. A new Stack Frame is created specifically for the main() method.

---

## Phase 5
### The Execution Engine (Step-by-Step)
The JVM Execution Engine reads the bytecode instructions from the Method Area and executes them. Here is exactly what happens in the Stack Frame for your addition logic:

Step A: int x = 5;

- bipush 5: Push the byte 5 onto the Operand Stack.

- istore_1: Pop 5 off the stack and store it in Local Variable Array at index 1 (which represents x).

Step B: int y = 10;

- bipush 10: Push 10 onto the Operand Stack.

- istore_2: Pop 10 off and store it in Local Variable Array at index 2 (which represents y).

Step C: int result = x + y;

- iload_1: Load the integer from local variable 1 (x) and push it onto the stack.

- iload_2: Load the integer from local variable 2 (y) and push it onto the stack.

- iadd: The math instruction.

   - The JVM pops the top two integers (5 and 10).

   - The CPU adds them.

   - The result 15 is pushed back onto the Operand Stack.

- istore_3: Pop 15 and store it in local variable 3 (result).

__Comparison Note__ : Notice how similar this is to Python, but Java uses specific types in the instructions (e.g., iadd for integer add), whereas Python's BINARY_ADD figures out the type at runtime.

---

## Phase 6
### Output & Native Interface (System.out.println)
This part is complex because it involves crossing the boundary from the Virtual Machine to the actual hardware.

8. __Accessing System.out__

    - getstatic: The JVM retrieves the static field out from the java.lang.System class. This is a reference to a PrintStream object.

9. __Invoking the Method__

    - iload_3: Pushes result (15) onto the stack.

    - invokevirtual: Calls the println(int) method on the PrintStream object.

10. __JNI (Java Native Interface)__ The println method eventually calls a Native Method (written in C).

    - The JVM uses JNI to bridge the gap to the OS libraries.

    - It calls the OS write function to send bytes to the standard output stream (stdout).

    - "15" appears on your terminal.


---

## Phase 7
### Teardown
11. __Frame Destruction__

    - The main method hits the closing brace }.

    - The Stack Frame for main is popped and destroyed. Local variables x, y, and result vanish.

12. __JVM Shutdown__

    - Since there are no other non-daemon threads running, the JVM shuts down.

    - Memory (Heap, Metaspace) is released back to the OS.

    - The process terminates code 0.









