
# ✅ Sum of Squares

***Difficulty: Medium***
### Problem:
### Given an ArrayList of integers, return the sum of the squares of all the numbers.

```java
public class SumSquares {
    public static int sumOfSquares(List<Integer> input) {
        // your code here
    }

    public static void main(String[] args) {
        List<Integer> input = Arrays.asList(1, 2, 3, 4);
        System.out.println(sumOfSquares(input));  // 30
    }
}
```

### Solution:

```java
public class SumSquares {
    public static int sumOfSquares(List<Integer> input) {
        return input.stream()
                    .map(n -> n * n)
                    .reduce(0, Integer::sum);
    }

    public static void main(String[] args) {
        List<Integer> input = Arrays.asList(1, 2, 3, 4);
        System.out.println(sumOfSquares(input));  // 30
    }
}
```