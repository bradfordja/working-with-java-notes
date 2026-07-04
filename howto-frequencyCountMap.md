# Frequency Count Map

***Difficulty: Easy***
### Problem:
### Given an array of strings, return a map containing the frequency of each word.

```java
public class WordFrequency {
    public static Map<String, Integer> getFrequency(String[] words) {
        // your code here
    }

    public static void main(String[] args) {
        String[] input = {"apple", "banana", "apple", "orange", "banana", "apple"};
        System.out.println(getFrequency(input));  // {apple=3, banana=2, orange=1}
    }
}
```

### Solution:

```java
public class WordFrequency {
    public static Map<String, Integer> getFrequency(String[] words) {
        Map<String, Integer> freqMap = new HashMap<>();
        for (String word : words) {
            freqMap.put(word, freqMap.getOrDefault(word, 0) + 1);
        }
        return freqMap;
    }

    public static void main(String[] args) {
        String[] input = {"apple", "banana", "apple", "orange", "banana", "apple"};
        System.out.println(getFrequency(input));  // {apple=3, banana=2, orange=1}
    }
}
```