Prob: https://leetcode.com/problems/3sum-closest/description/

Sol 1: Brute Force - Check all triplets
1. Run three nested loops to consider every triplet (i, j, k) where i < j < k.
2. For each triplet compute the sum and track the one with minimum absolute difference to target.

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int n = nums.length;
        int bestDiff = Integer.MAX_VALUE;
        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    int sum = nums[i] + nums[j] + nums[k];
                    int diff = Math.abs(sum - target);
                    if (diff < bestDiff) {
                        bestDiff = diff;
                        ans = sum;
                    }
                }
            }
        }
        return ans;
    }
}
```
Time complexity - O(n^3)
Space complexity - O(1)

Sol 2: Better - Sort + two pointers (basic)
1. Sort the array to enable two-pointer scanning for each fixed index.
2. For each i, use two pointers j = i+1 and k = n-1 and move them inward to find sums closer to target. Do not worry about duplicate-skipping — correctness holds for closest value.

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int n = nums.length;
        int bestDiff = Integer.MAX_VALUE;
        int ans = 0;

        for (int i = 0; i < n - 2; i++) {
            int j = i + 1, k = n - 1;
            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];
                int diff = Math.abs(sum - target);
                if (diff < bestDiff) {
                    bestDiff = diff;
                    ans = sum;
                }
                if (sum > target) k--;
                else j++;
            }
        }
        return ans;
    }
}
```
Time complexity - O(n^2)
Space complexity - O(1)

Sol 3: Optimal - Sorting + Two Pointers (with careful pointer movement and duplicate handling)
# Intuition
Sorting lets each two-pointer scan deterministically move toward sums closer to the target: increasing the left pointer raises the sum, decreasing the right pointer lowers it. Track the closest sum seen so far and update when a better one appears. Skipping identical values after moving pointers is optional for correctness but can reduce redundant checks.

1. Sort the array.
2. For each i from 0 to n-3, set j = i+1 and k = n-1.
3. While j < k, compute sum = nums[i] + nums[j] + nums[k], update answer if closer, then move pointers: if sum > target, k--, else j++.
4. Optionally skip duplicates after pointer moves to avoid repeated checks.

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int n = nums.length;
        int closest = Integer.MAX_VALUE;
        int ans = 0;

        for (int i = 0; i < n - 2; i++) {
            int sumNeeded = target - nums[i];
            int j = i + 1;
            int k = n - 1;

            while (j < k) {
                int sum2 = nums[j] + nums[k];
                int total = sum2 + nums[i];
                int diff = Math.abs(total - target);
                if (diff < closest) {
                    closest = diff;
                    ans = total;
                }

                if (sum2 == sumNeeded) {
                    // exact match
                    return target;
                } else if (sum2 > sumNeeded) {
                    k--;
                    // optional duplicate skip
                    while (j < k && k + 1 < n && nums[k] == nums[k + 1]) k--;
                } else {
                    j++;
                    // optional duplicate skip
                    while (j < k && j - 1 >= 0 && nums[j] == nums[j - 1]) j++;
                }
            }
        }

        return ans;
    }
}
```
Time complexity - O(n^2)
Space complexity - O(1)
