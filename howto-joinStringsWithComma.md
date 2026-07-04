# ✅ Join Strings with Comma

***Difficulty: Medium***
### Problem:
### Join a list of strings into a single string separated by commas.

```java
public class Joiner {
    public static String joinWithComma(List<String> input) {
        // your code here
    }

    public static void main(String[] args) {
        List<String> input = Arrays.asList("Java", "Python", "C++");
        System.out.println(joinWithComma(input));  // Java,Python,C++
    }
}
```

### Solution:

```java
public class Joiner {
    public static String joinWithComma(List<String> input) {
        return input.stream()
                    .collect(Collectors.joining(","));
    }

    public static void main(String[] args) {
        List<String> input = Arrays.asList("Java", "Python", "C++");
        System.out.println(joinWithComma(input));  // Java,Python,C++
    }
}
```
