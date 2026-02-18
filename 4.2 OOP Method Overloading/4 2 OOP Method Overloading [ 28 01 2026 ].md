# 4.2 OOPS Method Overloading [ 28/01/2026 ]

---

## Naming Conventions

- Class Names → Pascal Casing [ All words starts with Caps ]
- Method Names → Camel Casing
- Variable Names → Camel Csaing [ Snake Casing is also used by others ]

<aside>
💡

Method Signature → **combination of the method's name and its parameter list** (including the number, type, and order of the parameters)

- The return type, access modifiers, and the `throws` clause are NOT part of the method signature.
- The method signature is crucial
    - because the Java compiler uses it to uniquely identify methods within a class,
    - which is fundamental to the concept of method overloading.
</aside>

## Method Overloading

- Method overloading → Allowas **a class to have multiple methods with the Same Name but Different Parameters (number, type, or order)**
    - Return Type has NO impact on Method Overloading
    - Since data can be or can’t be Returned & caught, as it depends on User
- The compiler determines the correct method to call at compile time based on the arguments passed
- Hence, It is AKA Compile-Time Polymorphism (or) False Polymorphism [ Since It is NOT actual Polymorphism ]

```java
public class MethodOverloading {
    public static void main(String[] args) {
        Addition A = new Addition();
        System.out.println(A.add(10, 20));
        System.out.println(A.add(10, 20, 30));
        System.out.println(A.add(10, 20.5));
        System.out.println(A.add(10, 20, 30.5));
        System.out.println(A.add(10.5, 20));
        System.out.println(A.add(10, 20.5, 30));
    }
}

class Addition {
    int add (int a, int b) {
        return a + b;
    }
    int add (int a, int b, int c) {
        return a + b + c;
    }
    double add (int a, double b) {
        return a + b;
    }
    double add (int a, int b, double c) {
        return a + b + c;
    }
    double add (double a, int b) {
        return a + b;
    }
    double add (int a, double b, int c) {
        return a + b + c;
    }
}
```

Some Edge Cases

```java
public class MethodOverloading {
    public static void main(String[] args) {
       Addition A = new Addition();
       System.out.print(A.add(10, 20.5, 30));
    }
}

class Addition {
    double add (int a, double b, int c) {
        return a + b + c;
    }
}
```

```java
public class MethodOverloading {
    public static void main(String[] args) {
       Addition A = new Addition();
       System.out.print(A.add(10, 20, 30));
    }
}

class Addition {
    double add (int a, double b, int c) {
        return a + b + c;
    }
}
```

```java
public class MethodOverloading {
    public static void main(String[] args) {
       Addition A = new Addition();
       System.out.print(A.add(10, 20, 30.5));
    }
}

class Addition {
    double add (int a, double b, int c) {
        return a + b + c;
    }
}
```

Here, The Last case Fails

- Since, in case-II, int can be fitted in Double
But Couble can’t be fitted in Int as in case-III → Hence Fails

```java
public class MethodOverloading {
    public static void main(String[] args) {
        Addition A = new Addition();
        System.out.println(A.add(10, 20, 30));
    }
}

class Addition {
    double add (int a, int b, double c) {
        System.out.println("int, int, double");
        return a + b + c;
    }
    double add (double a, double b, double c) {
        System.out.println("int, double, double");
        return a + b + c;
    }
    double add (int a, double b, double c) {
        System.out.println("double, double, double");
        return a + b + c;
    }
}

// OUTPUT:
int, int, double
60.0
```

```java
public class MethodOverloading {
    public static void main(String[] args) {
        Addition A = new Addition();
        System.out.println(A.add(10, 20));
    }
}

class Addition {
    double add(int a, double b) {
        return a + b;
    }
    double add(double a, int b) {
        return a + b;
    }
}

```

***V.IMP case***

Here, 

- Compiler will get ambiguous to go for which method signature
- Hence, It will throw ERROR
But, if we have any one of the method only in class, then this will get executed

## Can we Overload `main` Method ?

- YES
- Then, which main method will be executed ?
    - JVM will execute/invole the default main Method → `main(String[] args)`

## Encapsulation

- Setter → A Method whose sole purpose is to Set Data [ for Private Instance Variable ]
- Getter → A Method whose sole purpose is to Get Data [ for Private Instance Variable ]
- These are Particularly used in Getting & Setting Private Variables of a Class from Another Class

```java
public class Encapsulation {
    public static void main(String[] args) {
        Employee emp1 = new Employee();
        emp1.setEmployeeId(152452);
        emp1.setEmployeeName("Jhonny English");
        System.out.println(emp1.getEmployeeId());
        System.out.println(emp1.getEmployeeName());
    }
}

class Employee {
    private int employeeId;
    private String employeeName;

    // Setter
    void setEmployeeId (int i) {
        employeeId = i;
    }
    void setEmployeeName (String i) {
        employeeName = i;
    }

    // Getter
    int getEmployeeId () {
        return  employeeId;
    }
    String getEmployeeName () {
        return  employeeName;
    }

}
```