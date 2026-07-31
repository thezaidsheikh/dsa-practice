# Striver Problem Note Template

Standard structure for problem notes in this folder. Striver-track notes follow Striver's sequencing and naming, covering Array problems (and beyond) with compact, solution-first writeups.

## How to Use This Template

1. First line: `Prob:` followed by the problem URL (LeetCode or Coding Ninjas).
2. Always write three solution tiers: `Sol 1: Brute Force`, `Sol 2: Better` (often labelled Optimized), `Sol 3: Optimal`.
3. Keep the numbered steps short and action-based.
4. Use Java. LeetCode-style notes use `class Solution`; Coding Ninjas-style notes use `public class Solution` with `static` methods (match the source).
5. End each solution with the time/space complexity line.
6. Preserve Striver naming and folder placement even when filenames aren't strict slugs.

## Template

````md
Prob: <problem-url>

Sol 1: Brute Force - <approach-name>

1. <step>
2. <step>

```java
class Solution {
    public <return-type> <method>(<args>) {
        // code
    }
}
```

Time complexity - O(...),
Space complexity - O(...)

Sol 2: Better - <approach-name>

1. <step>
2. <step>

```java
class Solution {
    // code
}
```

Time complexity - O(...),
Space complexity - O(...)

Sol 3: Optimal - <approach-name>

# Intuition

1. <step>
2. <step>

```java
class Solution {
    // code
}
```

Time complexity - O(...),
Space complexity - O(...)

```

```
````

## Filled Example (Longest Consecutive Sequence)

````md
Prob: https://www.codingninjas.com/studio/problems/longest-consecutive-sequence_759408

Sol 1: Brute Force - Using linear search

1. Take an element and loop through the array to find its next consecutive element.
2. If found, increment the count and keep going.
3. If not found, reset the count and move to the next starting element.
4. Return the maximum count.

```java
public class Solution {
    public static boolean linearSearch(int[] arr, int n, int prev) {
        for (int i = 0; i < n; i++) {
            if (arr[i] == prev + 1) return true;
        }
        return false;
    }
    public static int lengthOfLongestConsecutiveSequence(int[] arr, int N) {
        int longest = 1;
        for (int i = 0; i < N; i++) {
            int count = 1;
            int elem = arr[i];
            while (linearSearch(arr, N, elem)) {
                count++;
                elem += 1;
            }
            longest = Math.max(longest, count);
        }
        return longest;
    }
}
```
````

Time complexity - O(n^2),
Space complexity - O(1)

Sol 2: Better - Using sorting

1. Sort the array.
2. Walk through it, growing the current streak when the next element is consecutive.
3. Skip duplicates and reset the streak when the sequence breaks.
4. Track the maximum streak.

```java
import java.util.Arrays;

public class Solution {
    public static int lengthOfLongestConsecutiveSequence(int[] arr, int N) {
        Arrays.sort(arr);
        int longest = 1, current = 1;
        for (int i = 1; i < N; i++) {
            if (arr[i] == arr[i - 1]) continue;
            if (arr[i] == arr[i - 1] + 1) {
                current++;
            } else {
                current = 1;
            }
            longest = Math.max(longest, current);
        }
        return longest;
    }
}
```

Time complexity - O(n log n),
Space complexity - O(1)

Sol 3: Optimal - Using a HashSet

1. Store all elements in a set.
2. Only start counting from an element whose previous value (num - 1) is not in the set — it must be the start of a sequence.
3. Count consecutive elements from that start.
4. Return the longest streak.

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public static int lengthOfLongestConsecutiveSequence(int[] arr, int N) {
        Set<Integer> set = new HashSet<>();
        for (int num : arr) set.add(num);
        int longest = 1;
        for (int num : set) {
            if (!set.contains(num - 1)) {
                int currentNum = num;
                int current = 1;
                while (set.contains(currentNum + 1)) {
                    currentNum++;
                    current++;
                }
                longest = Math.max(longest, current);
            }
        }
        return longest;
    }
}
```

Time complexity - O(n),
Space complexity - O(n)

```

```
