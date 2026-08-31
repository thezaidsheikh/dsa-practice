Prob: https://leetcode.com/problems/4sum/description/

## Problem Summary
Given an array `nums` of `n` integers and a `target`, return all unique quadruplets `[nums[a], nums[b], nums[c], nums[d]]` such that `a < b < c < d` and their sum equals `target`. The answer must not contain duplicate quadruplets.

Sol 1: Brute Force - Check all quadruplets
1. Run four nested loops to pick every `(i, j, k, l)` with `i < j < k < l`.
2. If `nums[i] + nums[j] + nums[k] + nums[l] == target`, sort the quadruplet and add it to a `HashSet` (to ensure uniqueness).
3. Convert the set to the result list.

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        int n = nums.length;
        Set<List<Integer>> set = new HashSet<>();

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    for (int l = k + 1; l < n; l++) {
                        long sum = (long) nums[i] + nums[j] + nums[k] + nums[l];
                        if (sum == target) {
                            List<Integer> quad = Arrays.asList(nums[i], nums[j], nums[k], nums[l]);
                            Collections.sort(quad);
                            set.add(quad);
                        }
                    }
                }
            }
        }
        return new ArrayList<>(set);
    }
}
```
Time Complexity: O(n⁴ log n), O(n⁴) quadruplets generated, each insertion costs O(log m) in the set.
Space Complexity: O(m), where m is the number of unique quadruplets.

Sol 2: Better - Sort + fix one index + two-pointer two-sum
# Intuition
Sort the array so duplicate skipping becomes easy and two pointers work. Fix `i` and `j`, then use two pointers `k` and `l` to find pairs that sum to `target - nums[i] - nums[j]`. This reduces one loop from the brute force, going from O(n⁴) to O(n³).

1. Sort the array.
2. Iterate `i` from `0` to `n - 4`, skip duplicates for `i`.
3. Iterate `j` from `i + 1` to `n - 3`, skip duplicates for `j`.
4. Set `k = j + 1`, `l = n - 1`; move pointers based on current sum vs target.
5. On a match, record the quadruplet, move both pointers, and skip duplicates.

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        int n = nums.length;
        List<List<Integer>> ls = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1; j < n; j++) {
                if (j != i + 1 && nums[j] == nums[j - 1]) continue;

                int k = j + 1, l = n - 1;
                while (k < l) {
                    long sum = (long) nums[i] + nums[j] + nums[k] + nums[l];
                    if (sum == target) {
                        ls.add(Arrays.asList(nums[i], nums[j], nums[k], nums[l]));
                        k++;
                        l--;
                        while (k < l && nums[k] == nums[k - 1]) k++;
                        while (k < l && nums[l] == nums[l + 1]) l--;
                    } else if (sum > target) l--;
                    else k++;
                }
            }
        }
        return ls;
    }
}
```
Time Complexity: O(n³), sorting is O(n log n) and the two nested-pointer scan over `(i, j)` dominates.
Space Complexity: O(1) auxiliary (excluding the output and sorting overhead).

Sol 3: Optimal - Sort + fix two indices + two pointers (pruned)
# Intuition
After sorting, we fix two outer indices `i` and `j` and search for the remaining two with two pointers `k` and `l`. The critical insight is that since the array is sorted, the minimum sum achievable from index `j` onward is `nums[j] + nums[j+1] + nums[j+2] + nums[j+3]`, and the maximum is `nums[j] + nums[n-3] + nums[n-2] + nums[n-1]`. If the target is outside this window for a given `i`, we can skip ahead entirely. This pruning and careful duplicate skipping (only skip consecutive duplicates for the second fixed index `j`, not `j == i+1`) makes it a tight O(n³).

1. Sort the array.
2. Outer loop `i` from `0` to `n-4`, skip duplicate `i`.
3. Inner loop `j` from `i+1` to `n-3`; skip duplicates for `j` only when `j != i+1` (so the same value can still pair with a different `i`).
4. Two-pointer `k = j+1`, `l = n-1`: advance/shrink based on whether the sum is too low or too high.
5. On a match, record the quadruplet, advance both pointers, and skip over consecutive duplicates on both sides.

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> ls = new ArrayList<>();
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1; j < n; j++) {
                if (j != i + 1 && nums[j] == nums[j - 1]) continue;
                int k = j + 1;
                int l = n - 1;

                while (k < l) {
                    long sum = (long) nums[i] + nums[j] + nums[k] + nums[l];
                    if (sum == target) {
                        List<Integer> list = new ArrayList<>();
                        list.add(nums[i]);
                        list.add(nums[j]);
                        list.add(nums[k]);
                        list.add(nums[l]);
                        ls.add(list);
                        k++;
                        l--;
                        while (k < l && nums[k] == nums[k - 1]) k++;
                        while (k < l && nums[l] == nums[l + 1]) l--;
                    } else if (sum > target) l--;
                    else k++;
                }
            }
        }

        return ls;
    }
}
```
Time Complexity: O(n³), sorting is O(n log n) and the two-pointer scan per `(i, j)` pair runs in O(n).
Space Complexity: O(1) auxiliary (excluding output).

## Key Takeaways
- 4Sum is the direct extension of 3Sum: fix one more index, drop the innermost loop to a two-pointer scan, and the complexity goes from O(n³) to O(n³) (two fixed + two pointers = O(n² × n) = O(n³)).
- The `long` cast is mandatory: four `int` values can overflow, especially when the target is large.
- Duplicate skipping differs for the two outer indices vs the inner pointers: for the second fixed index `j`, use `j != i + 1` as the guard (not `j > i + 1`) so the value is allowed to appear with a different `i`.
- This pattern generalises to kSum: sort, fix (k-2) indices recursively, and two-pointer for the last two; time is O(n^(k-1)).
- Recognition cue: "find k elements that sum to target, no duplicates" -> sort, fix (k-2) indices, two-pointer tail; don't brute-force the full k-tuple.
