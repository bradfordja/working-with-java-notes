# Senior Interview Example

## Find the Second Largest Distinct Value

```java
import java.util.Arrays;

public class Main {

    public static int findSecondLargest(int[] nums) {
        return Arrays.stream(nums)
                .distinct()                  // Remove duplicates
                .boxed()                     // IntStream -> Stream<Integer>
                .sorted((a, b) -> b - a)     // Descending order
                .skip(1)                     // Skip largest value
                .findFirst()                 // Get second largest
                .orElseThrow(() ->
                    new IllegalArgumentException(
                        "Array must contain at least two distinct values"));
    }

    public static void main(String[] args) {

        int[] nums = {10, 5, 19, 8, 20, 15, 20};

        System.out.println(findSecondLargest(nums));
    }
}
```

### Output

```text
19
```

---

## Step-by-Step Explanation

### 1. Create a Stream

```java
Arrays.stream(nums)
```

Converts the array into an `IntStream`.

Example:

```java
[10, 5, 19, 8, 20, 15, 20]
```

---

### 2. Remove Duplicates

```java
.distinct()
```

Result:

```java
[10, 5, 19, 8, 20, 15]
```

---

### 3. Convert Primitive int to Integer

```java
.boxed()
```

Converts:

```java
IntStream
```

into:

```java
Stream<Integer>
```

Required because custom sorting works on objects.

---

### 4. Sort Descending

```java
.sorted((a, b) -> b - a)
```

Result:

```java
[20, 19, 15, 10, 8, 5]
```

---

### 5. Skip the Largest

```java
.skip(1)
```

Result:

```java
[19, 15, 10, 8, 5]
```

---

### 6. Get First Remaining Element

```java
.findFirst()
```

Result:

```java
19
```

---

### 7. Handle Empty Results

```java
.orElseThrow(...)
```

Throws an exception if fewer than two distinct values exist.

Example:

```java
[5, 5, 5]
```

Would throw:

```java
IllegalArgumentException:
Array must contain at least two distinct values
```

---

## Complexity Analysis

### Time Complexity

```text
distinct()  -> O(n)
sorted()    -> O(n log n)
skip()      -> O(1)
findFirst() -> O(1)

Overall: O(n log n)
```

### Space Complexity

```text
O(n)
```

because the Stream creates intermediate collections.

---

## Senior-Level Discussion

This solution is very readable and demonstrates:

- Streams
- Lambdas
- Sorting
- Functional Programming
- Optional Handling
- Exception Handling

However, a senior engineer should mention that this is not the most efficient solution because sorting requires:

```text
O(n log n)
```

A single-pass algorithm can solve the problem in:

```text
Time: O(n)
Space: O(1)
```

which is usually preferred for large datasets.

---

## Common Glider Follow-Up

Implement an O(n) solution without sorting.

```java
public static int findSecondLargest(int[] nums) {

    Integer largest = null;
    Integer secondLargest = null;

    for (int num : nums) {

        if (largest == null || num > largest) {
            secondLargest = largest;
            largest = num;
        } else if (num != largest &&
                   (secondLargest == null || num > secondLargest)) {
            secondLargest = num;
        }
    }

    if (secondLargest == null) {
        throw new IllegalArgumentException(
            "Array must contain at least two distinct values");
    }

    return secondLargest;
}
```

### Complexity

```text
Time: O(n)
Space: O(1)
```

This is typically the answer that earns the highest marks in a Senior Java Glider assessment.