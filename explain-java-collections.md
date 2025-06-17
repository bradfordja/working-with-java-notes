Types of collections in Java

Act as an export Java Developer,  you can expect interview questions that assess your in-depth understanding of collections types in Java.  Give me a list of all collections data types in Java with common interview code-snippet questions with answers  about working with each collection type.  Give simple explanation and sample uses-case if any or sample code-snippet for each Java collection.

Certainly, Java collections framework provides a variety of data structures for storing and manipulating groups of objects. Below are some common collection types in Java, accompanied by interview questions, explanations, and code snippets:

---

### 1. List

#### a) ArrayList
- **Question:** How to remove duplicates from an ArrayList?
- **Explanation:** `ArrayList` is an ordered, index-based collection.
- **Sample Code:**
  ```java
  ArrayList<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 2, 3, 4, 4, 5));
  List<Integer> uniqueList = new ArrayList<>(new HashSet<>(list));
  ```

#### b) LinkedList
- **Question:** How to add an element at the beginning and the end of a LinkedList?
- **Explanation:** `LinkedList` is useful for frequent insert/delete operations.
- **Sample Code:**
  ```java
  LinkedList<Integer> linkedList = new LinkedList<>();
  linkedList.addFirst(1);
  linkedList.addLast(2);
  ```

---

### 2. Set

#### a) HashSet
- **Question:** How to check if a HashSet contains a specific element?
- **Explanation:** `HashSet` is an unordered collection that doesn't allow duplicates.
- **Sample Code:**
  ```java
  HashSet<Integer> hashSet = new HashSet<>();
  hashSet.add(1);
  boolean containsOne = hashSet.contains(1);
  ```

#### b) TreeSet
- **Question:** How to get the smallest and largest elements in a TreeSet?
- **Explanation:** `TreeSet` is a sorted set implementation.
- **Sample Code:**
  ```java
  TreeSet<Integer> treeSet = new TreeSet<>(Arrays.asList(1, 2, 3));
  int smallest = treeSet.first();
  int largest = treeSet.last();
  ```

---

### 3. Map

#### a) HashMap
- **Question:** How to get all keys or values from a HashMap?
- **Explanation:** `HashMap` stores key-value pairs.
- **Sample Code:**
  ```java
  HashMap<String, Integer> map = new HashMap<>();
  map.put("One", 1);
  Set<String> keys = map.keySet();
  Collection<Integer> values = map.values();
  ```

#### b) TreeMap
- **Question:** How to get the first and last key from a TreeMap?
- **Explanation:** `TreeMap` sorts keys based on their natural ordering.
- **Sample Code:**
  ```java
  TreeMap<String, Integer> treeMap = new TreeMap<>();
  treeMap.put("One", 1);
  treeMap.put("Two", 2);
  String firstKey = treeMap.firstKey();
  String lastKey = treeMap.lastKey();
  ```

---

### 4. Queue

#### a) PriorityQueue
- **Question:** How to get the smallest element without removing it from a PriorityQueue?
- **Explanation:** `PriorityQueue` is a priority-based queue implementation.
- **Sample Code:**
  ```java
  PriorityQueue<Integer> priorityQueue = new PriorityQueue<>();
  priorityQueue.add(1);
  priorityQueue.add(2);
  int smallest = priorityQueue.peek();
  ```

#### b) ArrayDeque
- **Question:** How to use ArrayDeque as a stack?
- **Explanation:** `ArrayDeque` is a double-ended queue that can also be used as a stack.
- **Sample Code:**
  ```java
  ArrayDeque<Integer> stack = new ArrayDeque<>();
  stack.push(1);
  int top = stack.peek();
  ```

---

### 5. Other Collections

#### a) Vector
- **Question:** Why might you use Vector over ArrayList?
- **Explanation:** `Vector` is a thread-safe alternative to `ArrayList`.
- **Sample Code:**
  ```java
  Vector<Integer> vector = new Vector<>();
  vector.add(1);
  ```

#### b) Stack
- **Question:** How to pop an element from a Stack?
- **Explanation:** `Stack` is a Last-In-First-Out (LIFO) data structure.
- **Sample Code:**
  ```java
  Stack<Integer> stack = new Stack<>();
  stack.push(1);
  int popped = stack.pop();
  ```

I hope this comprehensive list helps you prepare for your interviews related to Java Collections.

#Java 
#CollectionsFramework 
