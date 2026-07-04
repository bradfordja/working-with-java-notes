# Group Anagrams

***Difficulty: Hard***
### Problem:
### Group a list of strings into groups of anagrams.

```java
public class GroupAnagrams {
    public static List<List<String>> group(List<String> words) {
        // your code here
    }

    public static void main(String[] args) {
        List<String> input = Arrays.asList("bat", "tab", "eat", "tea", "tan", "nat");
        System.out.println(group(input));
        // Example output: [[bat, tab], [eat, tea], [tan, nat]]
    }
}
```

### Solution:

```java
public class GroupAnagrams {
    public static List<List<String>> group(List<String> words) {
        Map<String, List<String>> map = new HashMap<>();
        for (String word : words) {
            char[] chars = word.toCharArray();
            Arrays.sort(chars);
            String sorted = new String(chars);
            map.computeIfAbsent(sorted, k -> new ArrayList<>()).add(word);
        }
        return new ArrayList<>(map.values());
    }

    public static void main(String[] args) {
        List<String> input = Arrays.asList("bat", "tab", "eat", "tea", "tan", "nat");
        System.out.println(group(input));
    }
}
```