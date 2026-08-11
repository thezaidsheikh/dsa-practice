Prob: https://leetcode.com/problems/minimum-size-subarray-sum/description/

Sol 1: Brute Force - Checking all subarrays
1. Run a loop (i) to pick the starting index of every subarray.
2. Run a loop (j) from i onward, building the sum incrementally.
3. Whenever the sum becomes >= target, record the length j - i + 1 and keep the minimum.
4. Return the minimum length found, or 0 if no subarray satisfies the condition.
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int minLen = Integer.MAX_VALUE;

        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += nums[j];
                if (sum >= target) {
                    minLen = Math.min(minLen, j - i + 1);
                    break;
                }
            }
        }

        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }
}
```
Time Complexity: O(n^2), in the worst case we explore all starting and ending pairs.
Space Complexity: O(1)

Sol 2: Better - Using prefix sum with binary search
1. Build a prefix sum array where prefix[i] = sum of elements from index 0 to i-1. It is sorted (non-decreasing) because all elements are positive.
2. For each starting index i, find the smallest j such that prefix[j] - prefix[i] >= target using binary search on the prefix array.
3. Update minLen with j - i if such a j exists.
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int[] prefix = new int[n + 1];

        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }

        int minLen = Integer.MAX_VALUE;
        for (int i = 0; i <= n; i++) {
            int need = prefix[i] + target;
            int lo = i, hi = n;
            while (lo < hi) {
                int mid = lo + (hi - lo) / 2;
                if (prefix[mid] < need) lo = mid + 1;
                else hi = mid;
            }
            if (lo <= n && prefix[lo] >= need) {
                minLen = Math.min(minLen, lo - i);
            }
        }

        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }
}
```
Time Complexity: O(n log n), one binary search per start index.
Space Complexity: O(n), for the prefix array.

Sol 3: Optimal - Sliding window (variable size)
# Intuition
Since all numbers are positive, growing the window can only increase the sum and shrinking it can only decrease the sum. This monotonicity lets us expand the right end until the window sum reaches target, then shrink the left end to minimize the length while keeping the sum >= target. Both ends only move forward, so each element is visited at most twice.

1. Expand the window by moving the right pointer and adding nums[right] to the running sum.
2. While the sum >= target, the current window is valid: update minLen, then shrink from the left by subtracting nums[left] and moving left forward.
3. Keep shrinking until the sum drops below target, then go back to expanding.
4. Return minLen, or 0 if it was never updated.
```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int minLen = Integer.MAX_VALUE;
        int sum = 0;
        int left = 0;

        for (int right = 0; right < n; right++) {
            sum += nums[right];
            while (sum >= target) {
                minLen = Math.min(minLen, right - left + 1);
                sum -= nums[left++];
            }
        }

        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }
}
```
Time Complexity: O(n), the left and right pointers each traverse the array once.
Space Complexity: O(1)

## Key Takeaways
- This is the classic variable-size sliding window problem.
- Recognition cue: positive numbers + "minimum length subarray with sum >= target" -> expand with right, shrink with left.
