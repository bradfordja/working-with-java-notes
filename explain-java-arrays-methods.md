Here is a copy-ready .md version.

Java Arrays Class Methods

The java.util.Arrays class provides utility methods for working with arrays, such as sorting, searching, comparing, copying, filling, and converting arrays to streams.

import java.util.Arrays;

⸻

1. Arrays.sort()

Sorts an array in ascending order.

int[] nums = {5, 2, 9, 1};
Arrays.sort(nums);
System.out.println(Arrays.toString(nums));

Output:

[1, 2, 5, 9]

⸻

2. Arrays.parallelSort()

Sorts an array using multiple threads. Useful for large arrays.

int[] nums = {9, 3, 7, 1, 5};
Arrays.parallelSort(nums);
System.out.println(Arrays.toString(nums));

Output:

[1, 3, 5, 7, 9]

⸻

3. Arrays.binarySearch()

Searches for a value in a sorted array.

int[] nums = {1, 3, 5, 7, 9};
int index = Arrays.binarySearch(nums, 7);
System.out.println(index);

Output:

3

Important: the array must be sorted first.

⸻

4. Arrays.toString()

Converts an array to a readable string.

int[] nums = {1, 2, 3};
System.out.println(Arrays.toString(nums));

Output:

[1, 2, 3]

⸻

5. Arrays.deepToString()

Converts a multi-dimensional array to a readable string.

int[][] matrix = {
    {1, 2},
    {3, 4}
};
System.out.println(Arrays.deepToString(matrix));

Output:

[[1, 2], [3, 4]]

⸻

6. Arrays.equals()

Compares two arrays for equal values.

int[] a = {1, 2, 3};
int[] b = {1, 2, 3};
System.out.println(Arrays.equals(a, b));

Output:

true

⸻

7. Arrays.deepEquals()

Compares multi-dimensional arrays.

int[][] a = {{1, 2}, {3, 4}};
int[][] b = {{1, 2}, {3, 4}};
System.out.println(Arrays.deepEquals(a, b));

Output:

true

⸻

8. Arrays.compare()

Compares two arrays lexicographically.

int[] a = {1, 2, 3};
int[] b = {1, 2, 4};
System.out.println(Arrays.compare(a, b));

Output:

-1

Explanation:

// -1 means first array is smaller
//  0 means arrays are equal
//  1 means first array is greater

⸻

9. Arrays.mismatch()

Finds the first index where two arrays are different.

int[] a = {1, 2, 3};
int[] b = {1, 2, 4};
int index = Arrays.mismatch(a, b);
System.out.println(index);

Output:

2

⸻

10. Arrays.fill()

Fills an array with the same value.

int[] nums = new int[5];
Arrays.fill(nums, 100);
System.out.println(Arrays.toString(nums));

Output:

[100, 100, 100, 100, 100]

⸻

11. Arrays.copyOf()

Copies an array to a new array.

int[] original = {1, 2, 3};
int[] copy = Arrays.copyOf(original, original.length);
System.out.println(Arrays.toString(copy));

Output:

[1, 2, 3]

You can also increase the size:

int[] bigger = Arrays.copyOf(original, 5);
System.out.println(Arrays.toString(bigger));

Output:

[1, 2, 3, 0, 0]

⸻

12. Arrays.copyOfRange()

Copies part of an array.

int[] nums = {10, 20, 30, 40, 50};
int[] result = Arrays.copyOfRange(nums, 1, 4);
System.out.println(Arrays.toString(result));

Output:

[20, 30, 40]

Note:

// Start index is included
// End index is excluded

⸻

13. Arrays.asList()

Converts an array to a fixed-size List.

String[] names = {"John", "Mary", "Alex"};
List<String> list = Arrays.asList(names);
System.out.println(list);

Output:

[John, Mary, Alex]

Important:

// You cannot add or remove items from this list
// But you can update existing values

⸻

14. Arrays.stream()

Converts an array into a Stream.

int[] nums = {10, 20, 30};
int sum = Arrays.stream(nums)
        .sum();
System.out.println(sum);

Output:

60

⸻

15. Arrays.setAll()

Sets array values using a lambda expression.

int[] nums = new int[5];
Arrays.setAll(nums, i -> i * 10);
System.out.println(Arrays.toString(nums));

Output:

[0, 10, 20, 30, 40]

⸻

16. Arrays.parallelSetAll()

Sets array values in parallel using a lambda expression.

int[] nums = new int[5];
Arrays.parallelSetAll(nums, i -> i * 2);
System.out.println(Arrays.toString(nums));

Output:

[0, 2, 4, 6, 8]

⸻

17. Arrays.spliterator()

Creates a Spliterator for traversing array elements.

String[] names = {"John", "Mary", "Alex"};
Arrays.spliterator(names)
        .forEachRemaining(System.out::println);

Output:

John
Mary
Alex

⸻

18. Arrays.hashCode()

Generates a hash code for an array.

int[] nums = {1, 2, 3};
System.out.println(Arrays.hashCode(nums));

Use case:

// Useful when arrays are used in hashing logic

⸻

19. Arrays.deepHashCode()

Generates a hash code for multi-dimensional arrays.

int[][] matrix = {
    {1, 2},
    {3, 4}
};
System.out.println(Arrays.deepHashCode(matrix));

⸻

Common Stream Examples with Arrays.stream()

Find Maximum

int[] nums = {10, 5, 20, 8};
int max = Arrays.stream(nums)
        .max()
        .orElseThrow();
System.out.println(max);

Output:

20

⸻

Find Minimum

int[] nums = {10, 5, 20, 8};
int min = Arrays.stream(nums)
        .min()
        .orElseThrow();
System.out.println(min);

Output:

5

⸻

Calculate Average

int[] nums = {10, 20, 30};
double avg = Arrays.stream(nums)
        .average()
        .orElseThrow();
System.out.println(avg);

Output:

20.0

⸻

Remove Duplicates

int[] nums = {1, 2, 2, 3, 3, 4};
Arrays.stream(nums)
        .distinct()
        .forEach(System.out::println);

Output:

1
2
3
4

⸻

Filter Even Numbers

int[] nums = {1, 2, 3, 4, 5, 6};
Arrays.stream(nums)
        .filter(n -> n % 2 == 0)
        .forEach(System.out::println);

Output:

2
4
6

⸻

Map Values

int[] nums = {1, 2, 3};
Arrays.stream(nums)
        .map(n -> n * n)
        .forEach(System.out::println);

Output:

1
4
9

⸻

Senior Interview Example

Find Second Largest Distinct Number

import java.util.Arrays;
public class Main {
    public static int findSecondLargest(int[] nums) {
        return Arrays.stream(nums)
                .distinct()
                .boxed()
                .sorted((a, b) -> b - a)
                .skip(1)
                .findFirst()
                .orElseThrow(() ->
                        new IllegalArgumentException(
                                "Array must contain at least two distinct values"));
    }
    public static void main(String[] args) {
        int[] nums = {10, 5, 19, 8, 20, 15, 20};
        System.out.println(findSecondLargest(nums));
    }
}

Output:

19

⸻

Senior Notes

// Arrays.sort() is simple and common.
// Arrays.parallelSort() may help with very large arrays.
// Arrays.binarySearch() requires sorted input.
// Arrays.equals() checks single-dimensional arrays.
// Arrays.deepEquals() checks multi-dimensional arrays.
// Arrays.stream() is common in modern Java interviews.
// Arrays.copyOf() is used when resizing arrays.
// Arrays.asList() creates a fixed-size list backed by the array.

⸻

Most Common Methods to Know for Interviews

Arrays.sort()
Arrays.binarySearch()
Arrays.toString()
Arrays.equals()
Arrays.fill()
Arrays.copyOf()
Arrays.copyOfRange()
Arrays.asList()
Arrays.stream()
Arrays.deepToString()
Arrays.deepEquals()