## 🧱 lambda expressions - Java 8, method references of lambda expressions
In Java 8, method references are a shorthand notation of lambda expressions used to refer to methods directly by their names. They improve readability and conciseness when a lambda expression simply calls an existing method.

⸻

### ✅ What is a Method Reference?

A method reference is a compact form of a lambda expression that calls an existing method.

📌 Syntax:
```java
ClassName::methodName
```

⸻

### ✅ Types of Method References

Type	Syntax	Description	Example
1. Static Method	ClassName::staticMethod	Refers to a static method	Math::max
2. Instance Method (particular object)	object::instanceMethod	Refers to a method of a particular object	System.out::println
3. Instance Method (any object of a particular type)	ClassName::instanceMethod	Refers to an instance method of a parameter	String::toLowerCase
4. Constructor Reference	ClassName::new	Creates a new instance	ArrayList::new


⸻

### ✅ Examples for Each Type

🔹 1. Static Method Reference
```java
import java.util.function.BiFunction;

public class Demo {
    public static int add(int a, int b) {
        return a + b;
    }

    public static void main(String[] args) {
        BiFunction<Integer, Integer, Integer> sum = Demo::add;
        System.out.println(sum.apply(10, 20)); // Output: 30
    }
}
```

⸻

🔹 2. Instance Method Reference (particular object)
```java
import java.util.function.Consumer;

public class Demo {
    public static void main(String[] args) {
        Consumer<String> printer = System.out::println;
        printer.accept("Hello, World!"); // Output: Hello, World!
    }
}
```

⸻

🔹 3. Instance Method Reference (of arbitrary object)
```java
import java.util.Arrays;
import java.util.List;

public class Demo {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        names.forEach(String::toUpperCase); // Invalid: does not modify in-place
        names.stream()
             .map(String::toUpperCase)
             .forEach(System.out::println);
    }
}
```

⸻

🔹 4. Constructor Reference
```java
import java.util.function.Supplier;

public class Demo {
    static class MyClass {
        MyClass() {
            System.out.println("Constructor called!");
        }
    }

    public static void main(String[] args) {
        Supplier<MyClass> supplier = MyClass::new;
        MyClass obj = supplier.get(); // Output: Constructor called!
    }
}
```

⸻

### ✅ Real-World Use Case: Sorting a List Using Method Reference
```java
import java.util.Arrays;
import java.util.List;
import java.util.Collections;

public class Demo {
    public static void main(String[] args) {
        List<String> fruits = Arrays.asList("Banana", "Apple", "Mango", "Orange");
        
        // Using lambda
        Collections.sort(fruits, (a, b) -> a.compareToIgnoreCase(b));
        
        // Using method reference
        Collections.sort(fruits, String::compareToIgnoreCase);

        fruits.forEach(System.out::println);
    }
}
```

⸻

### ✅ Summary

Expression	Method Reference
(a, b) -> a.compareTo(b)	String::compareTo
() -> new MyClass()	MyClass::new
x -> x.toLowerCase()	String::toLowerCase
x -> System.out.println(x)	System.out::println


⸻