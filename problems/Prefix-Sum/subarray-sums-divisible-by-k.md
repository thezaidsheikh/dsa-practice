Prob: https://leetcode.com/problems/subarray-sums-divisible-by-k/description/

## Problem Summary
Given an array of integers `nums` and an integer `k`, return the number of subarrays whose sum is divisible by `k`. (A subarray is a contiguous part of the array.)

Sol 1: Brute Force - Check every subarray
1. Pick every possible starting index `i` and every ending index `j >= i`.
2. Compute the sum of `nums[i..j]` and test whether it is divisible by `k`.
3. Count all such subarrays.

```java
class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        int n = nums.length;
        int count = 0;
        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += nums[j];
                if (sum % k == 0) count++;
            }
        }
        return count;
    }
}
```
Time Complexity: O(n²), all O(n²) subarrays, each summed in O(1) incremental time.
Space Complexity: O(1).

Sol 2: Better - Prefix sums + remainder frequency (two passes)
# Intuition
A subarray `nums[i..j]` has sum divisible by `k` exactly when two prefix sums share the same remainder modulo `k` (because `(prefix[j+1] - prefix[i]) % k == 0` iff `prefix[j+1] % k == prefix[i] % k`). Build the prefix-sum array once, then count how many prefix sums fall into each remainder bucket; any two from the same bucket form a valid subarray (choose 2).

1. Build `prefix[i]` = sum of `nums[0..i-1]` for `i = 0..n`.
2. Tally how many prefix sums have each remainder `(prefix[i] % k)` (handle negatives by adding `k`).
3. For each remainder bucket with frequency `f`, add `f * (f - 1) / 2` to the answer.

```java
class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        int n = nums.length;
        int[] prefix = new int[n + 1];
        for (int i = 0; i < n; i++) prefix[i + 1] = prefix[i] + nums[i];

        int[] freq = new int[k];
        for (int i = 0; i <= n; i++) {
            int rem = prefix[i] % k;
            if (rem < 0) rem += k;
            freq[rem]++;
        }

        int count = 0;
        for (int f : freq) count += f * (f - 1) / 2;
        return count;
    }
}
```
Time Complexity: O(n), one pass to build prefix sums and one to bucket remainders.
Space Complexity: O(n), for the prefix array (the remainder array is only O(k)).

Sol 3: Optimal - Running prefix sum with remainder map (one pass)
# Intuition
We don't need to store the whole prefix array. Keep a running `sum` and a frequency map of remainders seen so far. At index `i`, the remainder `rem = sum % k` (normalized to be non-negative). Every earlier prefix with the same remainder forms a divisible subarray with the current one, so we add `freq[rem]` to the result, then record the current remainder. Starting with `freq[0] = 1` counts a prefix sum itself being divisible by `k` (subarray starting at index 0).

The subtle point is the negative remainder: in Java, `-1 % 3 == -1`, but we need the mathematical modulo in `[0, k-1]`, so add `k` when `rem < 0`. Two prefix sums congruent in true-modulo terms always differ by a multiple of `k`, keeping the counting correct.

1. Initialize `sum = 0`, `res = 0`, and a map with `freq[0] = 1`.
2. For each element: add to `sum`, compute `rem = sum % k` and normalize negatives by `+= k`.
3. Add `freq.getOrDefault(rem, 0)` to `res`, then increment `freq[rem]`.
4. Return `res`.

```java
class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        int n = nums.length;
        Map<Integer, Integer> mp = new HashMap<>();

        int res = 0;
        int sum = 0;
        mp.put(0, 1);
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            int rem = sum % k;
            if (rem < 0) rem += k;
            int val = mp.getOrDefault(rem, 0);
            mp.put(rem, val + 1);
            res += val;
        }

        return res;
    }
}
```
Time Complexity: O(n), single pass; hash map operations are O(1) average.
Space Complexity: O(min(n, k)), the map holds at most `k` distinct remainders.

## Key Takeaways
- Subarray-sum divisible by `k` is the same prefix-sum-difference trick as Subarray Sum Equals K, but comparing congruences (remainders) instead of exact values — `count` choose-2 within each remainder bucket.
- Always normalize `sum % k` into `[0, k-1]`; Java's `%` can return negatives, which would break the remainder-matching.
- Seed the frequency map with `freq[0] = 1` so a prefix sum that is itself divisible by `k` (a subarray from index 0) is counted.
- Using a `Map` keeps space at O(min(n,k)); with a fixed small `k` you can swap it for an `int[k]` array for speed.
- Recognition cue: "number of subarrays with sum divisible/multiple of k" -> prefix sum + remainder frequency.
