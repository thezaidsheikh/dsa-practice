Prob: https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/

Sol 1: Brute Force - Checking all subarrays
1. Run a loop (i) that selects every possible starting index of the subarray.
2. Run a loop (j) that signifies the ending index of the subarray.
3. Calculate the sum of each subarray and track the maximum absolute sum seen so far.
4. Return the maximum absolute sum.

```java
class Solution {
    public int maxAbsoluteSum(int[] nums) {
        int n = nums.length;
        int ans = 0;

        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int sum = 0;
                for (int m = i; m <= j; m++) {
                    sum += nums[m];
                }
                ans = Math.max(ans, Math.abs(sum));
            }
        }
        return ans;
    }
}
```
Time Complexity: O(n^3), because for every pair of start and end indices we recompute the subarray sum from scratch.
Space Complexity: O(1)

Sol 2: Better - Building the sum incrementally
1. Notice that the sum of the subarray ending at j can be built incrementally from the subarray ending at j-1.
2. As j moves forward, add nums[j] to the running sum instead of recomputing from scratch.
3. Track the maximum absolute running sum across all subarrays.

```java
class Solution {
    public int maxAbsoluteSum(int[] nums) {
        int n = nums.length;
        int ans = 0;

        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += nums[j];
                ans = Math.max(ans, Math.abs(sum));
            }
        }
        return ans;
    }
}
```
Time Complexity: O(n^2), for each start index we extend the subarray while maintaining a running sum.
Space Complexity: O(1)

Sol 3: Optimal - Running max-Kadane and min-Kadane together
# Intuition
An absolute value is largest when the underlying sum sits at one of the two extremes: either the LARGEST possible subarray sum or the SMALLEST (most negative) one. Nothing in between can win. So the answer is simply max(|max subarray sum|, |min subarray sum|). Both of those are already solved problems — standard max-Kadane and min-Kadane — so we just run both DP states side by side in a single pass and take whichever extreme has the bigger magnitude at the end.

1. Initialize prev_max, prev_min, max, min all to nums[0].
2. For each element update both states with the usual "extend or restart" decision:
   prev_min = min(nums[i], nums[i] + prev_min), then shrink global min.
   prev_max = max(nums[i], nums[i] + prev_max), then grow global max.
3. Return max(abs(min), abs(max)).

```java
class Solution {
    public int maxAbsoluteSum(int[] nums) {
        int n = nums.length;
        int prev_min = nums[0];
        int prev_max = nums[0];
        int min = nums[0];
        int max = nums[0];

        for(int i = 1; i < n; i++) {
            prev_min = Math.min(nums[i], nums[i] + prev_min);
            min = Math.min(min, prev_min);

            prev_max = Math.max(nums[i], nums[i] + prev_max);
            max = Math.max(prev_max, max);
        }

        return Math.max(Math.abs(min), Math.abs(max));
    }
}
```
Time Complexity: O(n), single pass over the array running both Kadane states.
Space Complexity: O(1)

## Key Takeaways
- "Maximum absolute sum" decomposes instantly into two known problems: answer = max(Kadane-max, -Kadane-min). Recognize the decomposition instead of inventing a new algorithm.
- abs(x) is maximized only at the extremes of x's range — checking max and min covers everything.
- This note combines the two sibling notes in this folder: Maximum Subarray (the max half) and Smallest Sum Contiguous Subarray (the min half).
- Recognition cue: any "absolute value of subaggregate" extremum question -> optimize both directions and take the winner.
