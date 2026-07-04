# ✅ Count Strings by Length

***Difficulty: Medium***
### Problem:
### Count how many strings in an ArrayList have length greater than 3.

```java
public class StringLengthCount {
    public static long countLongStrings(List<String> input) {
        // your code here
    }

    public static void main(String[] args) {
        List<String> input = Arrays.asList("one", "three", "two", "four", "yes");
        System.out.println(countLongStrings(input));  // 2
    }
}
```

### Solution:

```java
public class StringLengthCount {
    public static long countLongStrings(List<String> input) {
        return input.stream()
                    .filter(s -> s.length() > 3)
                    .count();
    }

    public static void main(String[] args) {
        List<String> input = Arrays.asList("one", "three", "two", "four", "yes");
        System.out.println(countLongStrings(input));  // 2
    }
}
```