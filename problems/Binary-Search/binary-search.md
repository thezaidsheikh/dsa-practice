# Binary Search

Prob: https://leetcode.com/problems/binary-search/description/

## Problem Summary
Given a sorted array of integers and a target, return the index of the target, or -1 if it is not present. The array is sorted in ascending order.

Sol 1: Brute Force - Linear scan
1. Walk the whole array and check each element against the target.
2. Return the index on the first match, otherwise -1.
3. Works on any array but ignores the sorted-order guarantee.

```java
class Solution {
    public int search(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) return i;
        }
        return -1;
    }
}
```
Time complexity - O(n), worst case visits every element
Space complexity - O(1)

Sol 2: Optimal - Binary search with low/high pointers
# Intuition
Because the array is sorted, every comparison against the middle element tells us which half is worthless: if `nums[mid]` is bigger than the target, the target can only live in the left half; if smaller, only in the right half. Each step shrinks the search space in half, so only a logarithmic number of comparisons is ever needed.

1. Set `low = 0` and `high = nums.length - 1` as the search window.
2. While `low <= high`, compute `mid = (low + high) / 2`.
3. If `nums[mid] > target`, the target is in the left half — set `high = mid - 1`.
4. If `nums[mid] < target`, the target is in the right half — set `low = mid + 1`.
5. Otherwise `nums[mid] == target` — return `mid`.
6. If the loop ends with no match, return -1.

```java
class Solution {
    public int search(int[] nums, int target) {
        int low = 0, high = nums.length - 1;

        while (low <= high) {
            int mid = (low + high) / 2;
            if (nums[mid] > target) high = mid - 1;
            else if (nums[mid] < target) low = mid + 1;
            else return mid;
        }
        return -1;
    }
}
```
Time complexity - O(log n), each step discards half the remaining range
Space complexity - O(1), only pointer variables

## Key Takeaways
- Binary search needs a sorted input and an ordering rule that tells you which half to discard after each comparison.
- Use `low <= high` so single-element windows (where `low == high`) are still checked before giving up.
- Be careful the direction is preserved: `nums[mid] > target` must shrink `high`, `nums[mid] < target` must move `low`.
- Recognition cue: "sorted array" + "find an element / boundary" -> eliminate-half thinking before anything else.