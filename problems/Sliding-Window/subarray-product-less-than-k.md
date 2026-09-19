Prob: https://leetcode.com/problems/subarray-product-less-than-k/

## Problem Summary
Given a positive integer array `nums` and an integer `k`, return the number of contiguous subarrays where the product of all elements is strictly less than `k`. (All `nums[i]` are positive, so `k <= 1` yields 0.)

Sol 1: Brute Force - Count every qualifying subarray
1. Pick every starting index `i` and every ending index `j >= i`.
2. Maintain a running product of `nums[i..j]`; whenever it is `< k`, increment the count.
3. Return the total count.

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int n = nums.length;
        int count = 0;
        for (int i = 0; i < n; i++) {
            int product = 1;
            for (int j = i; j < n; j++) {
                product *= nums[j];
                if (product < k) count++;
                else break; // product only grows; no further j helps this i
            }
        }
        return count;
    }
}
```
Time Complexity: O(n²) worst case (when all products stay below k, no early break).
Space Complexity: O(1).

Sol 2: Better - Prefix products + binary search
# Intuition
Because every `nums[i] >= 1`, the running product over `nums[i..j]` is non-decreasing as `j` grows. Build a prefix-product array `P` (with `P[0] = 1`), so product of `nums[i..j]` = `P[j+1] / P[i]`. For a fixed `i`, the condition `P[j+1] / P[i] < k` means `P[j+1] < k * P[i]` — a value we can binary-search for in the monotonic `P` array, finding the farthest valid `j` in O(log n).

1. Build `P[i]` = product of `nums[0..i-1]` (use `long` to avoid overflow).
2. For each `i`, binary-search the largest index `m` with `P[m] < k * P[i]`; the number of valid subarrays starting at `i` is `m - i`.
3. Sum these counts.

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        if (k <= 1) return 0;
        int n = nums.length;
        long[] P = new long[n + 1];
        P[0] = 1;
        for (int i = 0; i < n; i++) P[i + 1] = P[i] * nums[i];

        int count = 0;
        for (int i = 0; i < n; i++) {
            long target = (long) k * P[i];
            int lo = i + 1, hi = n, m = i; // m = largest index with P[m] < target
            while (lo <= hi) {
                int mid = lo + (hi - lo) / 2;
                if (P[mid] < target) { m = mid; lo = mid + 1; }
                else hi = mid - 1;
            }
            count += (m - i);
        }
        return count;
    }
}
```
Time Complexity: O(n log n), one binary search per starting index.
Space Complexity: O(n), for the prefix-product array.

Sol 3: Optimal - Sliding window (two pointers)
# Intuition
Since all numbers are positive, growing the window only increases the product and shrinking only decreases it — the "valid product < k" property is monotonic in window size. So for each `right`, there is a unique smallest `left` such that `nums[left..right]` is valid; every subarray ending at `right` with a start in `[left, right]` also qualifies. We advance `left` only when the product gets too big, and the number of new valid subarrays ending at `right` is exactly `(right - left + 1)`. This visits each element at most twice.

1. If `k <= 1`, return 0 immediately (no positive product can be < 1 or < 0... actually < k impossible).
2. Keep `left = 0`, `product = 1`, `count = 0`.
3. For each `right`: multiply `product` by `nums[right]`. While `product >= k`, divide out `nums[left]` and increment `left`.
4. Add `(right - left + 1)` to `count` (all subarrays ending at `right` starting in `[left, right]`). Return `count`.

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int n = nums.length;
        int left = 0;
        int right = 0;

        if (k <= 1) return 0;

        int count = 0;
        int product = 1;

        for (right = 0; right < n; right++) {
            product *= nums[right];

            while (product >= k) {
                product = product / nums[left];
                left ++;
            }
            count += (right - left + 1);
        }
        return count;
    }
}
```
Time Complexity: O(n), each of `left` and `right` moves at most n times across the whole loop.
Space Complexity: O(1).

## Key Takeaways
- Monotonicity of the product (all positive) is what makes a sliding window valid: once the window exceeds `k`, only shrinking can fix it.
- For a window `[left, right]` that is valid, there are `(right - left + 1)` valid subarrays ending at `right` — count them all at once instead of enumerating.
- The `k <= 1` guard is essential: with positive integers the product is always `>= 1`, so no subarray can be `< k` when `k <= 1`.
- Use `long` for the product in practice (not shown above) to avoid `int` overflow on large inputs.
- Recognition cue: "count subarrays where product < k" with positive numbers -> sliding window / two pointers, not prefix sums (product has no nice subtraction inverse like sums do).
