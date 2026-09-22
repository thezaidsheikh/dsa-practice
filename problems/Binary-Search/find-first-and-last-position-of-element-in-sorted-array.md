# Find First and Last Position of Element in Sorted Array

Prob: https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/

## Problem Summary
Given a sorted array of integers and a target, return the first and last index where the target appears. If the target is absent, return `[-1, -1]`. Must run in O(log n).

Sol 1: Brute Force - Linear scan both directions
1. Scan from the left and record the first index equal to target.
2. Scan from the right and record the last index equal to target.
3. If neither is found, return `[-1, -1]`, otherwise return `[first, last]`.

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int first = -1, last = -1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) { first = i; break; }
        }
        for (int i = nums.length - 1; i >= 0; i--) {
            if (nums[i] == target) { last = i; break; }
        }
        return new int[]{first, last};
    }
}
```
Time complexity - O(n), two passes over the array
Space complexity - O(1)

Sol 2: Optimal - Two binary searches (leftmost then rightmost)
# Intuition
One plain binary search can find the target, but not the boundaries when the value repeats. To find the leftmost occurrence, whenever we hit the target we save the index and keep searching the left half with `high = mid - 1` — the search keeps going left until the first match. The rightmost is symmetric: save the index and keep searching the right half with `low = mid + 1`. Running both searches independently gives both boundaries in O(log n) each.

1. First search: set `low = 0`, `high = n - 1`.
2. While `low <= high`, compute `mid = (low + high) / 2`.
3. If `nums[mid] < target`, go right with `low = mid + 1`; if greater, go left with `high = mid - 1`.
4. On a match, save `mid` into `res[0]` and continue left with `high = mid - 1` to reach the leftmost occurrence.
5. Reset `low = 0`, `high = n - 1` and run the symmetric search.
6. On a match, save `mid` into `res[1]` and continue right with `low = mid + 1` to reach the rightmost occurrence.
7. Return the pair; `-1` values remain if the target never appeared.

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int n = nums.length;
        int low = 0;
        int high = n - 1;
        int[] res = {-1, -1};

        while (low <= high) {
            int mid = (low + high) / 2;
            if (nums[mid] < target) low = mid + 1;
            else if (nums[mid] > target) high = mid - 1;
            else {
                res[0] = mid;
                high = mid - 1;
            }
        }

        low = 0;
        high = n - 1;
        while (low <= high) {
            int mid = (low + high) / 2;
            if (nums[mid] < target) low = mid + 1;
            else if (nums[mid] > target) high = mid - 1;
            else {
                res[1] = mid;
                low = mid + 1;
            }
        }

        return res;
    }
}
```
Time complexity - O(log n), two binary searches each halving the range
Space complexity - O(1)

## Key Takeaways
- Boundaries of a range in a sorted array -> run binary search twice: keep pushing left for first, keep pushing right for last.
- The trick is not stopping on the first match — recording the match and continuing in the direction of the boundary is what finds the ends.
- If the target is absent, both loops never save an index and `[-1, -1]` is returned naturally.
- Recognition cue: "first and last occurrence / range of a value / count of occurrences" in sorted input -> use two lower-upper bound binary searches.