
# ✅ Filter Even Numbers

***Difficulty: Easy***
### Problem:
### Write a method that takes an ArrayList<Integer> and returns a new list containing only even numbers.

```java
public class FilterEven {
    public static List<Integer> filterEvenNumbers(List<Integer> input) {
        // your code here
    }

    public static void main(String[] args) {
        List<Integer> input = Arrays.asList(1, 2, 3, 4, 5, 6);
        System.out.println(filterEvenNumbers(input));  // [2, 4, 6]
    }
}
```

### Solution:

```java
public class FilterEven {
    public static List<Integer> filterEvenNumbers(List<Integer> input) {
        return input.stream()
                    .filter(n -> n % 2 == 0)
                    .collect(Collectors.toList());
    }

    public static void main(String[] args) {
        List<Integer> input = Arrays.asList(1, 2, 3, 4, 5, 6);
        System.out.println(filterEvenNumbers(input));  // [2, 4, 6]
    }
}
```