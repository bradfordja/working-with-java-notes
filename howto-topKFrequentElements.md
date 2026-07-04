# Top K Frequent Elements

***Difficulty: Medium***
### Problem:
### Return the top k most frequent elements from a list.

```java
public class TopKFrequent {
    public static List<String> topK(List<String> words, int k) {
        // your code here
    }

    public static void main(String[] args) {
        List<String> input = Arrays.asList("a", "b", "a", "c", "b", "a", "d");
        System.out.println(topK(input, 2));  // [a, b]
    }
}
```

### Solution:

```java
public class TopKFrequent {
    public static List<String> topK(List<String> words, int k) {
        Map<String, Integer> freqMap = new HashMap<>();
        for (String word : words)
            freqMap.put(word, freqMap.getOrDefault(word, 0) + 1);

        return freqMap.entrySet()
                      .stream()
                      .sorted((a, b) -> b.getValue().equals(a.getValue())
                              ? a.getKey().compareTo(b.getKey())
                              : b.getValue() - a.getValue())
                      .limit(k)
                      .map(Map.Entry::getKey)
                      .collect(Collectors.toList());
    }

    public static void main(String[] args) {
        List<String> input = Arrays.asList("a", "b", "a", "c", "b", "a", "d");
        System.out.println(topK(input, 2));  // [a, b]
    }
}
```