### 1. Primitive Data Types

#### a) byte
- **Question:** How do you initialize and use a `byte` variable?

- **Explanation:** `byte` is an 8-bit signed data type.
- **Sample Code:**

  ```java
  byte b = 10;
  byte c = 20;
  byte sum = (byte) (b + c); // Explicit cast needed
  ```

#### b) short
- **Question:** What is the difference between `byte` and `short`?

- **Explanation:** `short` is a 16-bit signed data type. It can hold larger values than `byte`.
- **Sample Code:**

  ```java
  byte b = 127;
  short s = b;
  s += 1; // No explicit cast needed here
  ```
