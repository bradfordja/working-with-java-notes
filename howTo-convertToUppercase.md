
# ✅ Convert to Uppercase

***Difficulty: Easy***
### Problem:
### Convert all strings in an ArrayList to uppercase.

```java
public class ToUpper {
    public static List<String> convertToUpperCase(List<String> input) {
        // your code here
    }

    public static void main(String[] args) {
        List<String> input = Arrays.asList("java", "stream", "api");
        System.out.println(convertToUpperCase(input));  // [JAVA, STREAM, API]
    }
}
```

### Solution:

```java
public class ToUpper {
    public static List<String> convertToUpperCase(List<String> input) {
        return input.stream()
                    .map(String::toUpperCase)
                    .collect(Collectors.toList());
    }

    public static void main(String[] args) {
        List<String> input = Arrays.asList("java", "stream", "api");
        System.out.println(convertToUpperCase(input));  // [JAVA, STREAM, API]
    }
}
```