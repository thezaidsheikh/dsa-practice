Prob: https://leetcode.com/problems/find-pivot-index/description/

## Problem Summary
Given an array of integers `nums`, the pivot index is the index where the sum of all numbers strictly to the left of the index equals the sum of all numbers strictly to the right. Return the leftmost pivot index, or `-1` if none exists. (For index 0, the left sum is 0; for the last index, the right sum is 0.)

Sol 1: Brute Force - Recompute both sides for every index
1. For each index `i`, loop through indices `0..i-1` to sum the left side, then loop through `i+1..n-1` to sum the right side.
2. If left == right, return `i`.
3. If no index satisfies it, return -1.

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            int left = 0;
            for (int j = 0; j < i; j++) left += nums[j];

            int right = 0;
            for (int j = i + 1; j < n; j++) right += nums[j];

            if (left == right) return i;
        }
        return -1;
    }
}
```
Time Complexity: O(n²), two inner scans for each of the n indices.
Space Complexity: O(1), only scalar variables.

Sol 2: Better - Prefix sums array
# Intuition
Recomputing left sums from scratch at every index is wasteful — each left sum differs from the previous one by exactly `nums[i-1]`. Build a prefix-sum array once, then for any index `i`: left sum = `prefix[i]` (sum of `0..i-1`) and right sum = `total - prefix[i+1]` (sum of `i+1..n-1`). This collapses the inner loops into O(1) lookups.

1. Compute `prefix[i]` = sum of `nums[0..i-1]` for every index (prefix[0] = 0).
2. Compute total sum of the array.
3. For each index `i`: left = `prefix[i]`, right = `total - prefix[i] - nums[i]`; return `i` if they match.

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int n = nums.length;
        int[] prefix = new int[n + 1];
        for (int i = 0; i < n; i++) prefix[i + 1] = prefix[i] + nums[i];

        int total = prefix[n];
        for (int i = 0; i < n; i++) {
            int left = prefix[i];
            int right = total - prefix[i + 1];
            if (left == right) return i;
        }
        return -1;
    }
}
```
Time Complexity: O(n), one pass to build the prefix array and one to check each index.
Space Complexity: O(n), for the prefix-sum array.

Sol 3: Optimal - Running left sum with total sum
# Intuition
We don't need the whole prefix array. Keep a running `left` sum as we scan: at index `i`, `left` already holds the sum of everything before `i`, and the right sum is simply `total - nums[i] - left`. Update `left` by adding `nums[i]` for the next iteration. One pass over the data, no extra array.

1. Compute the total sum of `nums` once.
2. Initialize `left = 0`. For each index `i`: set right = `total - nums[i] - left`; if `left == right` return `i`; otherwise add `nums[i]` to `left` and continue.
3. If the loop finishes, return -1.

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int n = nums.length;
        int left = 0;
        int sum = 0;

        for (int i = 0; i < n; i++) {
            sum += nums[i];
        }

        for (int i = 0; i < n; i++) {
            if (i == 0) left = 0;
            else left += nums[i - 1];
            int right = sum - nums[i] - left;
            if (left == right) return i;
        }

        return -1;
    }
}
```
Time Complexity: O(n), one pass for the total and one pass for the check.
Space Complexity: O(1), only `left` and `sum` scalars.

## Key Takeaways
- A pivot index is just the spot where `leftSum == total - nums[i] - leftSum`; rearrange to `2*leftSum + nums[i] == total` if you prefer a single comparison.
- The optimization is recognizing that left sums overlap: maintain one running total instead of recomputing. This is the core prefix-sum habit.
- Edge cases: index 0 has left = 0, last index has right = 0 — both fall out naturally from the formula.
- Recognition cue: "sum to the left equals sum to the right" / "split array into equal halves" -> prefix/running sum before nested loops.
