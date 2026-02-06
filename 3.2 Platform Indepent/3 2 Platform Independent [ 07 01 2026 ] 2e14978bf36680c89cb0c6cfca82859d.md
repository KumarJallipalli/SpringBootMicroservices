# 3.2 Platform Independent [ 07/01/2026 ]

---

<aside>
💡

JAVA is Platform Independet

JAVA follows WORA [ Write Once, Run Anywhere ]

</aside>

## Platform

- Platform is combination of CPU [ Hardware ] & OS [ Software ]
    - like [ Intel + Windows ], [Intel + MacOS], [ M1 + MacOS ] etc..
- But for us Software Engineers, Platform Refers to OS only [ Not CPU + OS ]
- Hence, Platform → OS [ Linux, Windows, Mac etc.. ]

## Platform Dependency

- Suppose an Application is developed on a Windows OS
    - This App is compiled & an Executable version of app is generated →  [ MLL code ]
    - Now, Windows OS will be able to RUN this `.exe` file [ as this is MLL code for windows OS ]
    - Testing is done & App is working fine → Production ready
- Now I want to make this App available for all Users
    - So, I will upload this `.exe` file to Cloud
    - Why `.exe` file ?
        - Because, `.exe` file is secured & production ready
        - We shouldn’t upload the original Source code file
- Now all the different users will download into their own Machines
    - Linux
        - In Linux machines, this `.exe` file won’t work
        - Since, MLL code for Linux is NOT `.exe`
    - Mac
        - In Mac machines, this `.exe` file won’t work
        - Since, MLL code for Mac is NOT `.exe`
    - Windows.
        - In Windows machines, this `.exe` file works
        - Since, MLL code for Windows is `.exe`
- Because,
    - System relies on system-specific Architecture, libraries, APIs, etc.. to convert the souce code to MLL & Executing that MLL code
    - As these are different on each platform, same `.exe` file won’t work on all Machines
- Hence we can say,
    - The App we developed will only work on Windows OS
    - i.e., App works only on WIndows Platform

Platform Dependent → **software/Code that only runs on a specific operating system (OS) or hardware architecture** (like Windows on x86)

- because it relies on system-specific libraries, APIs, or machine code that isn't transferable, requiring recompilation for different environments
- e.g., C/C++

![Screenshot From 2026-01-07 09-15-30.png](3%202%20Platform%20Independent%20%5B%2007%2001%202026%20%5D/Screenshot_From_2026-01-07_09-15-30.png)

## How Java became Platform Independent

- For Java, A Special Compiler is made called Java Complier
    - This Java Compiler is a Hybrid Compiler
    - Instead of Converting HLL to MLL, this will convert to an Intermediate Code
- Java App/Code will be Compiled by Java Compiler & Generates an Intermediate Code called Byte Code
    - Java Code → `Javac` → Byte Code
- This Byte Code is Platform Independent
- This Byte Code is neither HLL Code nor MLL Code
- It requires a special softare called JVM to convert this Byte Code to MLL
    - Byte Code → `JVM` → MLL
- JVM is Platform Dependent
- JVM is an Interpreter
- Unlike other Languages,
    - where we upload MLL code,
    - In Java, we will upload Byte Code
    - All Machines will download this Byte Code [ instead of MLL ]
    - Now, to Execute this Byte Code, we require JVM [ needs to be available on respective machines ]
    - JVM will execute Byte Code & Generate MLL on the respective local Machine
    - This MLL code will be executed by CPU
- In this way,
    - MLL will be generated in the respective local Machine [ using JVM & Byte Code ]
    - Hence, Java became Plateform Independent
- So, Java is Platform Independent
    - AKA Java follows WORA
    - AKA java is Portable

![Screenshot From 2026-01-07 11-44-30.png](3%202%20Platform%20Independent%20%5B%2007%2001%202026%20%5D/Screenshot_From_2026-01-07_11-44-30.png)