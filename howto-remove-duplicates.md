

```java
  ArrayList<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 2, 3, 4, 4, 5));
  List<Integer> uniqueList = new ArrayList<>(new HashSet<>(list));
```

✅ Code Breakdown
```java
ArrayList<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 2, 3, 4, 4, 5));
```

•	Creates an ArrayList of Integer values:
[1, 2, 2, 3, 4, 4, 5]
•	The list includes duplicate values (2 and 4).

⸻

```java
List<Integer> uniqueList = new ArrayList<>(new HashSet<>(list));
```

•	new HashSet<>(list): Converts the list into a HashSet, which automatically removes duplicates.
### Example result (order is not guaranteed): [1, 2, 3, 4, 5]
•	new ArrayList<>(...): Converts the HashSet back to an ArrayList.

⸻

📌 Key Concepts

Concept	Explanation
•	ArrayList	Allows duplicates and maintains order
•	HashSet	No duplicates, but unordered
•	Arrays.asList()	Creates a fixed-size list from array-like args
•	new ArrayList<>(...)	Converts a Collection to a mutable list


⸻

✅ Result

## After execution:
	•	list contains: [1, 2, 2, 3, 4, 4, 5]
	•	uniqueList contains: [1, 2, 3, 4, 5] (duplicates removed, order not guaranteed)

⸻

⚠️ Note

## If maintaining the original order (without duplicates) matters, you should use LinkedHashSet instead:
```java
List<Integer> uniqueOrderedList = new ArrayList<>(new LinkedHashSet<>(list));
```

⸻

### Let me know if you’d like this turned into a reusable utility method.