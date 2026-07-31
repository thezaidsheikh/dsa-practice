# Leetcode-150 Problem Note Template

Standard structure for problem notes in this folder. `Leetcode-150/` stores compact notes for Top Interview 150 problems — solution-focused, minimal prose, one file per problem.

## How to Use This Template

1. First line: `Prob:` followed by the LeetCode problem URL.
2. Always write three solution tiers: `Sol 1: Brute Force`, `Sol 2: Better`, `Sol 3: Optimal`.
3. Keep the numbered steps short and action-based.
4. Write Java code in a `class Solution` (LeetCode style).
5. End each solution with the time/space complexity line.
6. Do not skip tiers — the Brute → Better → Optimal progression is the point of the note.

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

## Filled Example (Majority Element)

````md
Prob: https://leetcode.com/problems/majority-element/description

Sol 1: Brute Force - Using 2 loops

1. First loop tracks the element whose count we are checking.
2. Second loop counts how many times that element occurs.
3. If the count is greater than n/2, return the element.

```java
class Solution {
    public int majorityElement(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            int count = 0;
            for (int j = 0; j < n; j++) {
                if (nums[j] == nums[i]) count++;
            }
            if (count > n / 2) return nums[i];
        }
        return -1;
    }
}
```
````

Time complexity - O(n^2),
Space complexity - O(1)

Sol 2: Better - Using a hash map

1. Store the frequency of each number in a hash map.
2. Return the first number whose frequency exceeds n/2.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> freq = new HashMap<>();
        int n = nums.length;
        for (int num : nums) {
            int count = freq.getOrDefault(num, 0) + 1;
            freq.put(num, count);
            if (count > n / 2) return num;
        }
        return -1;
    }
}
```

Time complexity - O(n),
Space complexity - O(n)

Sol 3: Optimal - Using Moore's voting

1. Treat two different elements as canceling each other out.
2. The element with more than n/2 votes can never be fully canceled.
3. Find the surviving candidate and return it.

```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 1, elem = nums[0];
        for (int i = 1; i < nums.length; i++) {
            if (count == 0) {
                elem = nums[i];
                count = 1;
            } else if (nums[i] == elem) {
                count++;
            } else {
                count--;
            }
        }
        return elem;
    }
}
```

Time complexity - O(n),
Space complexity - O(1)

```

```
