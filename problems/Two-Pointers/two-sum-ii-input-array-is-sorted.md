# Two Sum II - Input Array Is Sorted

Prob: https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/

Sol 1: Brute Force - Using two nested loops
1. For each index i, check every index j after it.
2. If numbers[i] + numbers[j] == target, return {i+1, j+1}.
3. Since the array is sorted, we can do better — but this works for any array.

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int n = numbers.length;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (numbers[i] + numbers[j] == target) {
                    return new int[]{i + 1, j + 1};
                }
            }
        }
        return new int[]{-1, -1};
    }
}
```
Time complexity - O(n^2),
Space complexity - O(1)

Sol 2: Better - Using a hash map
1. Store each element with its index in a map.
2. For each element, check if (target - numbers[i]) was seen before.
3. The map gives O(1) lookup, trading space for speed.
4. Works even on an unsorted array.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] numbers, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < numbers.length; i++) {
            int complement = target - numbers[i];
            if (map.containsKey(complement)) {
                return new int[]{map.get(complement) + 1, i + 1};
            }
            map.put(numbers[i], i);
        }
        return new int[]{-1, -1};
    }
}
```
Time complexity - O(n),
Space complexity - O(n)

Sol 3: Optimal - Using two pointers
# Intuition
The array is sorted, so we can place one pointer at the start and one at the end. The sum of these two extremes tells us exactly which way to move:
- If the sum is too big, we need a smaller number, so move the right pointer left.
- If the sum is too small, we need a bigger number, so move the left pointer right.
Because the order is sorted, moving a pointer never skips a valid pair, so one pass is enough.

1. Initialize index1 at the start (0) and index2 at the end (length - 1).
2. While index1 < index2, compute the current sum.
3. If the sum equals the target, break out.
4. If the sum is greater than the target, decrement index2 (reduce the sum).
5. If the sum is less than the target, increment index1 (increase the sum).
6. Return the 1-indexed positions.

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int length = numbers.length;
        int index1 = 0;
        int index2 = length - 1;

        while (index1 < index2) {
            int sum = numbers[index1] + numbers[index2];
            if (sum == target) break;
            else if (sum > target) index2--;
            else index1++;
        }
        return new int[]{index1 + 1, index2 + 1};
    }
}
```
Time complexity - O(n), single pass with two pointers
Space complexity - O(1), no extra space used
