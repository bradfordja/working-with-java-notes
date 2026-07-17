Here’s a deep dive into **exception handling in Core Java**, focused on how it works, why it exists, and how to use it well.

**What Is an Exception?**

An exception is an abnormal event that disrupts the normal flow of a program.

Example:

```java
int result = 10 / 0;
```

This throws:

```text
ArithmeticException: / by zero
```

Instead of letting the program crash abruptly, Java lets you **handle** the problem using exception handling.

---

**Exception Hierarchy**

At the top:

```text
Throwable
├── Error
└── Exception
    ├── Checked Exceptions
    └── RuntimeException
        └── Unchecked Exceptions
```

Important types:

```java
Throwable
```

Base class for everything that can be thrown.

```java
Error
```

Serious JVM-level problems. Usually you do **not** handle these.

Examples:

```java
OutOfMemoryError
StackOverflowError
```

```java
Exception
```

Recoverable application-level problems.

Examples:

```java
IOException
SQLException
ClassNotFoundException
```

```java
RuntimeException
```

Programming mistakes or invalid runtime conditions.

Examples:

```java
NullPointerException
IllegalArgumentException
ArithmeticException
IndexOutOfBoundsException
```

---

**Checked vs Unchecked Exceptions**

**Checked exceptions** are checked at compile time.

You must either:

1. Handle them with `try-catch`
2. Declare them with `throws`

Example:

```java
import java.io.FileReader;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            FileReader reader = new FileReader("data.txt");
        } catch (IOException e) {
            System.out.println("File could not be opened.");
        }
    }
}
```

`IOException` is checked, so Java forces you to deal with it.

Common checked exceptions:

```java
IOException
SQLException
FileNotFoundException
ClassNotFoundException
InterruptedException
```

**Unchecked exceptions** are not checked at compile time.

Example:

```java
String name = null;
System.out.println(name.length());
```

This throws:

```java
NullPointerException
```

Java does not force you to catch unchecked exceptions because they usually indicate bugs.

Common unchecked exceptions:

```java
NullPointerException
ArithmeticException
ArrayIndexOutOfBoundsException
IllegalArgumentException
IllegalStateException
NumberFormatException
```

---

**Basic try-catch**

```java
try {
    int result = 10 / 0;
    System.out.println(result);
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero.");
}
```

Flow:

```text
try block starts
exception occurs
remaining try code is skipped
matching catch block runs
program continues
```

---

**Multiple catch Blocks**

```java
try {
    int[] numbers = {1, 2, 3};
    System.out.println(numbers[5]);
} catch (ArithmeticException e) {
    System.out.println("Math error.");
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Invalid array index.");
} catch (Exception e) {
    System.out.println("Something went wrong.");
}
```

Order matters.

More specific exceptions must come before general ones.

Wrong:

```java
try {
    // code
} catch (Exception e) {
    // catches everything
} catch (IOException e) {
    // unreachable code
}
```

This will not compile because `Exception` already catches `IOException`.

---

**Multi-Catch**

If several exceptions have the same handling logic:

```java
try {
    // risky code
} catch (IOException | NumberFormatException e) {
    System.out.println("Input problem: " + e.getMessage());
}
```

Useful when you do not need separate handling for each exception type.

---

**finally Block**

A `finally` block runs whether an exception happens or not.

```java
try {
    System.out.println("Opening resource");
    int result = 10 / 2;
} catch (ArithmeticException e) {
    System.out.println("Math error");
} finally {
    System.out.println("Cleanup happens here");
}
```

Used for cleanup:

```java
FileReader reader = null;

try {
    reader = new FileReader("data.txt");
} catch (IOException e) {
    System.out.println("File error");
} finally {
    if (reader != null) {
        try {
            reader.close();
        } catch (IOException e) {
            System.out.println("Could not close file");
        }
    }
}
```

But modern Java usually prefers `try-with-resources`.

---

**try-with-resources**

Introduced in Java 7.

It automatically closes resources that implement `AutoCloseable`.

```java
try (FileReader reader = new FileReader("data.txt")) {
    int data = reader.read();
    System.out.println(data);
} catch (IOException e) {
    System.out.println("File error: " + e.getMessage());
}
```

This is cleaner than manually closing in `finally`.

Works with:

```java
FileInputStream
FileReader
BufferedReader
Scanner
Connection
PreparedStatement
ResultSet
```

Example:

```java
try (
    BufferedReader br = new BufferedReader(new FileReader("data.txt"))
) {
    String line = br.readLine();
    System.out.println(line);
} catch (IOException e) {
    System.out.println("Could not read file.");
}
```

---

**throw Keyword**

Use `throw` to manually throw an exception.

```java
public static void validateAge(int age) {
    if (age < 18) {
        throw new IllegalArgumentException("Age must be at least 18");
    }
}
```

Usage:

```java
validateAge(15);
```

This throws:

```java
IllegalArgumentException
```

Use `throw` when your method detects invalid data or a business-rule violation.

---

**throws Keyword**

Use `throws` in a method signature to declare that the method might throw an exception.

```java
public static void readFile() throws IOException {
    FileReader reader = new FileReader("data.txt");
}
```

Then the caller must handle it:

```java
public static void main(String[] args) {
    try {
        readFile();
    } catch (IOException e) {
        System.out.println("File problem.");
    }
}
```

Difference:

```java
throw
```

Actually throws an exception.

```java
throws
```

Declares that a method may throw one.

---

**Custom Exceptions**

You can create your own exception class.

Checked custom exception:

```java
class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```

Usage:

```java
class BankAccount {
    private double balance = 100;

    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException("Not enough balance");
        }

        balance -= amount;
    }
}
```

Caller:

```java
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();

        try {
            account.withdraw(200);
        } catch (InsufficientFundsException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

Unchecked custom exception:

```java
class InvalidAgeException extends RuntimeException {
    public InvalidAgeException(String message) {
        super(message);
    }
}
```

Use checked exceptions when the caller can reasonably recover.

Use unchecked exceptions for programming errors or invalid usage.

---

**Common Exception Methods**

```java
e.getMessage()
```

Returns the error message.

```java
e.printStackTrace()
```

Prints full stack trace.

```java
e.getCause()
```

Returns the underlying cause.

```java
e.toString()
```

Returns exception class plus message.

Example:

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println(e.getMessage());
    e.printStackTrace();
}
```

---

**Stack Trace**

A stack trace shows where the exception happened.

Example:

```text
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at Main.divide(Main.java:8)
    at Main.main(Main.java:3)
```

Read it from top to bottom:

```text
Main.divide(Main.java:8)
```

The actual exception occurred here.

```text
Main.main(Main.java:3)
```

This method called `divide`.

---

**Exception Propagation**

If a method does not catch an exception, it propagates to the caller.

```java
public static void methodA() {
    methodB();
}

public static void methodB() {
    int result = 10 / 0;
}
```

Flow:

```text
methodB throws ArithmeticException
methodB does not catch it
exception goes to methodA
methodA does not catch it
exception goes to main
program terminates
```

Handled version:

```java
public static void main(String[] args) {
    try {
        methodA();
    } catch (ArithmeticException e) {
        System.out.println("Handled in main");
    }
}
```

---

**Best Practices**

Catch the most specific exception possible:

```java
catch (FileNotFoundException e)
```

Better than:

```java
catch (Exception e)
```

Do not swallow exceptions:

```java
catch (Exception e) {
}
```

Bad because the error disappears.

Prefer meaningful messages:

```java
throw new IllegalArgumentException("Price cannot be negative");
```

Avoid using exceptions for normal control flow:

```java
try {
    int value = Integer.parseInt(input);
} catch (NumberFormatException e) {
    // okay for invalid parsing
}
```

But do not use exceptions as a replacement for ordinary `if` checks.

Use `try-with-resources` for files, streams, sockets, and database connections.

Log the exception or handle it, but usually do not do both unless needed.

Bad:

```java
catch (IOException e) {
    e.printStackTrace();
    throw e;
}
```

This may produce duplicate error logs.

---

**Interview-Level Summary**

Exception handling in Java provides a structured way to handle runtime problems. Java exceptions are objects derived from `Throwable`. Exceptions are divided into checked and unchecked categories. Checked exceptions must be handled or declared, while unchecked exceptions usually represent programming mistakes. Java provides `try`, `catch`, `finally`, `throw`, `throws`, and `try-with-resources` to manage exceptional conditions.

A strong Java developer should know when to catch an exception, when to propagate it, when to create custom exceptions, and how to avoid hiding or overusing exceptions.