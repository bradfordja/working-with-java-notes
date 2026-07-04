# Remove Duplicates from List

**Difficulty: Easy**
### Problem:
### Given a list of integers, return a list with duplicates removed, preserving insertion order.

```java
public class RemoveDuplicates {
    public static List<Integer> removeDupes(List<Integer> input) {
        // your code here
    }

    public static void main(String[] args) {
        List<Integer> input = Arrays.asList(1, 2, 2, 3, 1, 4);
        System.out.println(removeDupes(input));  // [1, 2, 3, 4]
    }
}
```

### Solution:

```java
public class RemoveDuplicates {
    public static List<Integer> removeDupes(List<Integer> input) {
        return new ArrayList<>(new LinkedHashSet<>(input));
    }

    public static void main(String[] args) {
        List<Integer> input = Arrays.asList(1, 2, 2, 3, 1, 4);
        System.out.println(removeDupes(input));  // [1, 2, 3, 4]
    }
}
```