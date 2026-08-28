Prob: https://www.naukri.com/code360/problems/longest-subarray-with-sum-k_5713505

## Problem Summary
Given an array `nums` (may contain negative numbers and zeros) and an integer `k`, return the length of the longest subarray whose sum equals `k`. If no such subarray exists, return 0.

Sol 1: Brute Force - Check every subarray
1. Pick every starting index `i` and every ending index `j >= i`.
2. Maintain a running sum of `nums[i..j]` as `j` advances; when the sum equals `k`, update the maximum length.
3. Return the longest length found.

```java
import java.util.*;

public class Solution {
    public static int getLongestSubarray(int[] nums, int k) {
        int n = nums.length;
        int max = 0;
        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += nums[j];
                if (sum == k) max = Math.max(max, j - i + 1);
            }
        }
        return max;
    }
}
```
Time Complexity: O(n²), all O(n²) subarrays, each extended in O(1).
Space Complexity: O(1).

Sol 2: Better - Prefix-sum array + first-occurrence map
# Intuition
For a subarray `nums[i..j]`, `sum(i..j) = prefix[j+1] - prefix[i]`. We want `prefix[j+1] - prefix[i] == k`, i.e. `prefix[i] == prefix[j+1] - k`. Build the prefix array, then for each prefix value record the EARLIEST index it appeared; for the current prefix, look up how far back the needed value first occurred and measure the length. Using `long` avoids integer overflow on large inputs.

1. Build `prefix[i]` = sum of `nums[0..i-1]` for `i = 0..n` (as `long`).
2. Keep a map from prefix value to its first index.
3. For each index `i`: if `prefix[i] == k`, length is `i` (subarray from start); else if `prefix[i] - k` was seen first at index `p`, length is `i - p`. Update max; store `i` only if `prefix[i]` is new.

```java
import java.util.*;

public class Solution {
    public static int getLongestSubarray(int[] nums, int k) {
        int n = nums.length;
        long[] prefix = new long[n + 1];
        for (int i = 0; i < n; i++) prefix[i + 1] = prefix[i] + nums[i];

        Map<Long, Long> map = new HashMap<>();
        int max = 0;
        for (int i = 0; i <= n; i++) {
            if (prefix[i] == k) max = Math.max(max, i);
            long need = prefix[i] - k;
            if (map.containsKey(need)) max = Math.max(max, i - (int) (long) map.get(need));
            if (!map.containsKey(prefix[i])) map.put(prefix[i], (long) i);
        }
        return max;
    }
}
```
Time Complexity: O(n), two passes over the data with O(1) map ops.
Space Complexity: O(n), prefix array plus the map.

Sol 3: Optimal - Running prefix sum with first-occurrence map (one pass)
# Intuition
We don't need the whole prefix array. Keep a running `sum` and a map of the FIRST index where each prefix sum appeared. At index `i`, if `sum == k` the subarray from `0..i` works (length `i+1`). Otherwise, if an earlier prefix `sum - k` was first seen at index `p`, the subarray `p+1..i` sums to `k`, giving length `i - p`. Store `sum` only the first time it appears so lengths stay maximal. `long` guards against overflow.

1. Initialize `sum = 0`, `max = 0`, and a map (no need to pre-seed `0` since the `sum == k` check covers the start-at-0 case).
2. For each `i`: add `nums[i]` to `sum`. If `sum == k`, update max with `i + 1`.
3. Let `rem = sum - k`; if `rem` is in the map, update max with `i - map.get(rem)`.
4. If `sum` is not yet in the map, store its index `i`. Return max.

```java
import java.util.* ;
import java.io.*; 

public class Solution {
    public static int getLongestSubarray(int []nums, int k) {
        Map<Long, Long> map = new HashMap<>();
        int max = 0;
        long sum = 0;

        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (sum == k) {
                max = Math.max(max, i + 1);
            }
            long rem = sum - k;
            if (map.get(rem) != null) {
                int len = (int)(i - map.get(rem));
                max = Math.max(max, len);
            }
            if (map.get(sum) == null) map.put(sum, (long) i);
        }
        return max;
    }
}
```
Time Complexity: O(n), single pass; map operations are O(1) average.
Space Complexity: O(n), the map holds at most one entry per distinct prefix sum.

## Key Takeaways
- This is the "longest length" variant of Subarray Sum Equals K: instead of counting, store the FIRST occurrence of each prefix sum so lengths are maximized.
- Always store the first (not latest) index — overwriting would shrink the measured subarray length.
- The `sum == k` check (or seeding `map[0] = -1`) handles the subarray that begins at index 0; both styles are valid.
- Use `long` for the running sum and map keys — negatives plus large arrays overflow a 32-bit `int`.
- With negatives, the two-pointer window technique does NOT work; prefix-sum + hash map is the right tool.
- Recognition cue: "longest subarray with sum exactly k" (especially with negatives) -> prefix sum + first-occurrence map.
