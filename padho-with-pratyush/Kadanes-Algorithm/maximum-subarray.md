Prob: https://leetcode.com/problems/maximum-subarray/description/

Sol 1: Brute Force - Checking all subarrays
1. Run a loop (i) that selects every possible starting index of the subarray.
2. Run a loop (j) that signifies the ending index of the subarray.
3. Calculate the sum of each subarray and track the maximum sum seen so far.
4. Return the maximum sum.

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int n = nums.length;
        int max = Integer.MIN_VALUE;

        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int sum = 0;
                for (int m = i; m <= j; m++) {
                    sum += nums[m];
                }
                max = Math.max(max, sum);
            }
        }
        return max;
    }
}
```
Time Complexity: O(n^3), because for every pair of start and end indices we recompute the subarray sum from scratch.
Space Complexity: O(1)

Sol 2: Better - Building the sum incrementally
1. Notice that the sum of the subarray ending at j can be built incrementally from the subarray ending at j-1.
2. As j moves forward, add nums[j] to the running sum instead of recomputing from scratch.
3. Track the maximum running sum across all subarrays.

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int n = nums.length;
        int max = Integer.MIN_VALUE;

        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += nums[j];
                max = Math.max(max, sum);
            }
        }
        return max;
    }
}
```
Time Complexity: O(n^2), for each start index we extend the subarray while maintaining a running sum.
Space Complexity: O(1)

Sol 3: Optimal - Kadane's algorithm
# Intuition
For every index i, `best` holds the maximum sum of a subarray ending exactly at i. A subarray ending at i either starts fresh at nums[i], or extends the best subarray ending at i-1 by adding nums[i]. Extending only makes sense if the previous `best` is positive — a negative prefix would only drag the sum down, so we drop it and start over. Since the array is guaranteed non-empty, every element alone forms a valid subarray, so tracking the global maximum of all `best` values handles all-negative arrays correctly too.

1. Initialize `best` to 0 and `res` to Integer.MIN_VALUE.
2. For each element, update `best = max(nums[i] + best, nums[i])`: either extend the previous subarray or start a new one at nums[i].
3. Update the global answer `res` with the current `best`.
4. Return `res`.

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int n = nums.length;
        int best = 0;
        int res = Integer.MIN_VALUE;

        for (int i = 0; i < n; i++) {
            best = Math.max(best + nums[i], nums[i]);
            res = Math.max(best, res);
        }

        return res;
    }
}
```
Time Complexity: O(n), single pass over the array.
Space Complexity: O(1)

## Key Takeaways
- Kadane's DP state: `best` = max sum of a subarray ending at the current index; the answer is the max of that state over all indices.
- The core decision at each element: extend the running subarray or restart at the current element.
- This is the mirror problem of Smallest Sum Contiguous Subarray in this folder — identical structure with min/max swapped.
