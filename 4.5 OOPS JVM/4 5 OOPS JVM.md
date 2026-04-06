# 4.5 OOPS JVM [ 01/02/2026 ]

---

WKT, 

- Demo.java → Executed by Java Compiler [ javac ] & returns Byte Code [ Demo.class ]
- Demo.class → Executed by JVM & returns Machine Code
- Hence,
    - Java Compiler will check for Compilation Errors
    - JVM will check for Runtime Errors

## JVM

The **Java Virtual Machine (JVM)** is the engine that:

1. Loads your compiled `.class` files
2. Stores data needed while your program runs
3. Executes bytecode
4. Manages memory & garbage collection

High-level flow → `.java → javac → .class → JVM → machine code → OS`

## JVM Architecture

The Java Virtual Machine (JVM) architecture consists of three core subsystems that enable platform-independent code execution: 

1. **Class Loader Subsystem** [ Loads & verifies classes ]
2. **Runtime Data Areas** [ Stores program data]
3. **Execution Engine** [ Runs bytecode]

**1. Class Loader Subsystem**

**“How classes enter the JVM”**

Before anything runs, JVM must **load classes into memory**.

- **Loading:**
    - Loads class files
        - Bootstrap Class Loaders → Core Java classes (`java.lang.*`)
        - Extension/Platform Class Loaders → JDK extension libraries
        - Application Class Loaders → Classes from your project
    - Reads Class Files
- **Linking:**
    - **Verification** → Verifies bytecode integrity & Checks bytecode safety checks → Security
    - **Preparation** → prepares static fields by Allocating Memory for static fields/variables
    - **Resolution** → converts symbolic references to direct references
- **Initialization:**
    - Assigns static fields.
    - Executes static blocks & static initializers

**2. Runtime Data Areas**

“Where JVM stores stuff while your program runs”

These are memory areas organized by the JVM to store data during execution.

- **Method Area:** Stores class-level data & Code [ Class metadata, Static Variables/Methods, ***Method bytecode***, Runtime constant pool ]
- **Heap Area:** Stores all objects and their corresponding instance variables.
- **Stack Area:** Stores Stack frames (local variables, partial results, method calls) for each thread.
- **PC Registers:** Holds address of Current Instruction & Every Instruction [ Needed for thread switching ]
- **Native Method Stack:** Stores native method information [ methods marked `native` which are in C/C++ ]

**3. Execution Engine**

**“Where bytecode actually runs”**

Once bytecode is loaded → Execution Engine takes over.

The Execution Engine executes the bytecode stored in the Runtime Data Areas.

- **Interpreter:**
    - Reads and executes bytecode instructions line-by-line,
    - Executes immediately but slow to execute. [ as needed to execute every line & everytime ]
- **JIT Compiler (Just-In-Time):**
    - Compiles frequently called bytecode (hot spots) to native machine code & then Cached for reuse → improves performance.
    - It Supports Interpreter [ to boost the Performance, by Compiling & Caching the frequently used bytecode ]
    - JVM starts slow → becomes fast over time (**warm-up effect**)
- **Garbage Collector (GC):** Automatically removes unused/referrence-less objects from Heap → Frees Memory

**4. Other Components**

- **JNI (Java Native Interface):** Allows Java to talk with Native/System libraries (C/C++) & OS-level APIs
- **Native Method Libraries:** Libraries required for execution.

These components combined allow Java to run code on any device without recompilation. 

`Class Loader → Runtime Data Areas → Execution Engine → OS`

![image.png](4%205%20OOPS%20JVM%20%5B%2001%2002%202026%20%5D/image.png)

## When does Memory for Static Variable is Allocated ?

- Memory for static variables is Allocated in METHOD AREA during Linking Stage [ of Class Loading ]
    - Memory for static variables is allocated **once at the start of program execution (or class loading)** and
    - Resides in a special data segment, typically the Methoda Area [ Class Area ] rather than the stack.
    - This memory persists for the entire lifetime of the application [ from **start of program till end of Program ]**
- Key Points:
    - **Shared Memory:** Static variables are shared among all instances of a class.
    - **Single Initialization:** They are initialized only once.
    - **No Re-allocation:** The memory does not change during execution.

## When does Static Block will be Executed ?

- Static Block will be executed during Linking Stage [ of Initialization Phase by Class Loader Subsystem ]
    - A static block in Java is executed automatically by JVM **only ONCE, when the class is first loaded into memory**.

## Is Java an Interpreted or Compiled Language ?

- 1st Java code is converted to Byte Code using Java Compiler
- Then Byte Code is Converted to Machine Code using Internal JVM Interpreter
- Hence, Java is Both Interpreted & Compiled Language

## Static

- Static Memebers [ Variables/Blocks/Methods ] are Object Independent → Doesn’t Require Object to Access it
    - Hence, Static Members are called Class Members [ as these are Not Instance Members ]
- Static variables and static initialization blocks `static { ... }` in Java are executed **when the class is initialized/Loaded by JVM**
    - The execution of these elements is a one-time event during the lifecycle of the class loader.

```jsx
class Display {
    static int a, b, c;

    static {
        System.out.println("Display Class Static Block Initialization");
        a = 10;
        b = 20;
        c = 30;
    }

    static void display () {
        System.out.println("Static Method");
        System.out.println("a " + a);
        System.out.println("b " + b);
        System.out.println("c " + c);
    }

}

public class Demo {
    public static void main(String[] args) {
        Display.display();
    }
}
```

```jsx
// OUTPUT
Display Class Static Block Initialization
Static Method
a 10
b 20
c 30
```

Execution of static elements, is triggered by the JVM in specific scenarios:

- **First Instance Creation:** When the first object instance of the class is created (e.g., `MyClass myObject = new MyClass();`).
- **Accessing Static Members:** When a static method of the class is invoked or a static field (variable) is accessed or assigned a value for the first time [1]. (Note: accessing compile-time constant static fields, like `public static final int CONSTANT = 5;` might not trigger initialization as their values can be inlined by the compiler).
- **Explicit Reference:** When the class is explicitly referenced using methods like `Class.forName("com.example.MyClass")` if the `initialize` parameter is set to true [1].
- **Subclass Initialization:** When a subclass is initialized, the JVM ensures the superclass is initialized first.

In this case,

- Execution of static elements is triggered while Accessing Static Members [ static method of the class is invoked ]

EXERCISE - 1

- What is the Output ?

```jsx
class Display {
    static int a, b, c;

    static {
        System.out.println("Static Block Initialization");
        a = 10;
        b = 20;
        c = 30;
    }

    static void display () {
        System.out.println("Static Method");
        System.out.println("a " + a);
        System.out.println("b " + b);
        System.out.println("c " + c);
    }

}

public class Demo {
    public static void main(String[] args) {
    }
}
```

```jsx
// OUTPUT
BLANK
```

WHY..?

- In Java, a class is **loaded only when it is actually used**.
- Here,
    - `main()` is **empty**
    - The `Display` class is **never referenced**
    - So the JVM never loads `Display`
    - And because the class is never loaded:
        - the **static block does NOT run**
        - the **static method is never called**

- Static block runs → **when class is loaded**
- Class loads → **only when referenced**
- Your code → **never references `Display`**
- Hence → **blank output**

SOLUTION

```jsx
public class Demo {
    public static void main(String[] args) {
        // Execution of static elements by Accessing Static Members - 1 [ Accessing Variable ]
        System.out.println(Display.a);

        // Execution of static elements by Accessing Static Members - 2 [ method invoking ]
        Display.display();
        
        // Execution of static elements by Referencing Class Explicitly
        Class.forName("Display");
    }
}
```

<aside>
💡

NOTE:

- **Static blocks execute only when the class is loaded**, not when the program starts.
- Static variables are allocated memory and assigned their initial default values first, then their explicit initializers run in order.
- Code within `static { ... }` blocks is executed sequentially

The primary purpose of static blocks is 

- to perform complex initialization for static variables [ that cannot be done in a single line of code ]
</aside>

EXERCISE - 2

- WKT, Java Execution begins from `main` method
- Then which one gets executed FIRST

```jsx
// which one gets executed FIRST
public class Main {
    static int a;

    static {
        a = 10;
        System.out.println("a " + a);
        System.out.println("Static Block Initialization");
    }

    public static void main(String[] args) {
        System.out.println("Main Block Execution");
    }
}
```

```jsx
// OUTPUT
a 10
Static Block Initialization
Main Block Execution
```

WHY..?

- Because, `Main` Class will gets loaded 1st [ during Class Loader Subsystem ] & then JVM executes `main` method [ During Execution Engine ]
    - As we have used Static Block & Variables,
    - These gets executed during Class Loading itself
    - Hence, the Output is like that

EXERCISE - 3

```jsx
class Display {
    static int a, b, c;
    int x, y, z;

    static {
        System.out.println("Static Block Initialization");
        a = 10;
        b = 20;
        c = 30;
    }

    static void display () {
        System.out.println("Static Method");
        System.out.println("a " + a);
        System.out.println("b " + b);
        System.out.println("c " + c);
    }

    Display () {
        System.out.println("Constructor Execution");
        a++;
    }

    {
        System.out.println("Non-Static Block Initialization");
        x = 40;
        y = 50;
        z = 60;
    }

    void nonStaticDisplay () {
        System.out.println("Non-Static Method");
        System.out.println("x " + x);
        System.out.println("y " + y);
        System.out.println("z " + z);
        System.out.println("a " + a);
    }

}

public class Demo {
    public static void main(String[] args) {
        Display.display();

        Display d1 = new Display();
        d1.nonStaticDisplay();
    }
}
```

Flow:

1. `Demo` Class is Loaded
2. JVM executes `main` method
3. `Display.display();` → Display Class is Loaded
    - The **Class Loader** loads `Display` into the **Method Area**.
    - Memory gets allocated in Method Area [ **AKA Class-Level Memory** ]
        - Class metadata (fields, methods, bytecode)
        - **Static variables → `static int a, b, c;`**
        - **Static methods** → `static void display()`
4. Static Block Execution (Runs ONCE)
5. Static Method Call → `Display.display();`
    - JVM executes Static Method
6. Object Creation: `new Display()`
    - This triggers **object creation** in the **Heap**.
    - Memory allocated in Heap
        - Instance variables: → `int x, y, z;`
        - Each object gets **its own copy**. [ i.e., A Separate Copy ]
7. Instance Initialization Block (Runs BEFORE constructor)
    - Before the constructor runs, JVM executes the **non-static block**:
8. Constructor Execution
9. Non-Static Method Call → `d1.nonStaticDisplay();`

OUTPUT

```jsx
Static Block Initialization
Static Method
a 10
b 20
c 30
Non-Static Block Initialization
Constructor Execution
Non-Static Method
x 40
y 50
z 60
a 11
```

Method Area (Class-Level Memory)

| Member | Stored Once? |
| --- | --- |
| `static int a, b, c` | ✅ Yes |
| `static display()` | ✅ Yes |
| Bytecode & class metadata | ✅ Yes |

Heap (Object-Level Memory) → For **each `new Display()`**:

| Member | Stored Per Object? |
| --- | --- |
| `int x, y, z` | ✅ Yes |
| Non-static block data | ✅ Yes |

Stack (Execution Memory) → For each method call:

- `main()`
- `display()`
- `Display()` constructor
- `nonStaticDisplay()`

Each method call gets its **own stack frame**

<aside>
💡

### 📌 Important rules

- Static blocks run **only ONCE per class**
    - They run **before any static method or object creation**
- Static variables get memory ONCE [ in Method Area ] & shared by all objects
- Instance Initialization Block ALWAYS Runs BEFORE Constructor
- Instance Initialization Block
    - gets Executed Every time an object is created &
    - ALWAYS Executed BEFORE Constructor
    - that too, After DEFAULT Values are Assigned
</aside>

<aside>
💡

## Golden Rules Summary

- **Static block** → runs once when class loads
- **Static members** → class-level, Shared Memory
- **Non-static block** → runs before constructor
- **Constructor** → runs after non-static block
- **Instance variables** → separate copy per object
</aside>

Exercise - 4 [ WHat if we create 2 Objects ]

```jsx
public class Demo {
    public static void main(String[] args) {
        Display.display();

        Display d1 = new Display();
        d1.nonStaticDisplay();

        Display d2 = new Display();
        d1.nonStaticDisplay();
    }
}
```

Memory Allocation

```jsx
┌──────────────────────────┐
│        Method Area       │
│──────────────────────────│
│ Class: Display           │
│                          │
│ static int a = 10        │
│ static int b = 20        │
│ static int c = 30        │
│                          │
│ static void display()    │
│ Display() constructor    │
│ nonStaticDisplay()       │
│                          │
│ Static Block Bytecode    │
└──────────────────────────┘
```

Loaded **once** when `Display.display()` is called.

- Only **one copy**
- Shared by **all objects**
- Exists until JVM shuts down

```jsx
┌──────────────────────────┐
│           Heap           │
│──────────────────────────│
│ Object: Display@101      │
│ x = 40, y = 50, z = 60   │
│──────────────────────────│
│ Object: Display@102      │
│ x = 40, y = 50, z = 60   │
└──────────────────────────┘
```

Allocated when → `Display d1 = new Display();`

Each object gets:

- Its **own copy** of `x, y, z`
- Same static variables (from Method Area)

PROOF → Static Is Shared

If you add this inside constructor → `a++;`

```jsx
┌──────────────────────────┐
│           Stack          │
│──────────────────────────│
│ nonStaticDisplay()       │
│──────────────────────────│
│ Display() constructor    │
│──────────────────────────│
│ display()                │
│──────────────────────────│
│ main()                   │
└──────────────────────────┘

```

Stack frames:

- Created on method call
- Destroyed after method ends
- Store local variables & references

EXERCISE-5

- Create a Program to check No. of Objects created for a Class
- HINT → Static Variable

## Why Can’t we Access Instance Variables inside Static members [ Block/Method ]

- Instance Variables requires Object Creation
    - While Creation of Object only, Memory gets created for Instance Variables
- But Static Members are Independent of Objects → Accessed directly through Class name

## Why Can’t we Access Static Variables inside Non-Static members [ Block/Method ]

- Since Static members are created, initialized & executed during Class Loading Stage
- it’s Memory gets created during this stage only
- Hence, we can use those anywhere [ inside Static Members & Non-Static Members ]

## Does static variables memory allocated in stack or heap ?

- Static variables are allocated in **neither the stack nor the heap**.
- Their memory is typically allocated in a dedicated region of memory called the **data segment**
    - (sometimes AKA **static data area** or similar names depending on the programming language and system architecture)

Key points regarding static variable memory allocation:

- **Data Segment:** This region is managed by the OS and is separate from the stack and heap
- **Lifetime:** Their lifetime is the entire duration of the program's execution [ i.e., Memory allocated when program starts & deallocated when program ends ]
- **Scope:** While they have a global lifetime, their visibility (scope) might be limited to a specific function or file, depending on how they are declared

In contrast:

- **Stack:**
    - Used for local variables and function call management;
    - memory is automatically managed and freed when a function returns
- **Heap:**
    - Used for dynamic memory allocation requested by the programmer (e.g., using `malloc` or `new`);
    - memory must be manually freed by the programmer