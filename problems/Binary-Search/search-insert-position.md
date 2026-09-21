# Search Insert Position

Prob: https://leetcode.com/problems/search-insert-position/description/

## Problem Summary
Given a sorted array of distinct integers and a target, return the index where the target is present, or the index where it would be inserted to keep the array sorted.

Sol 1: Brute Force - Linear scan
1. Walk the array until you find the target or the first element greater than the target.
2. Return that index; if the target is larger than every element, return the array length.
3. Works on any array but ignores the sorted-order guarantee.

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] >= target) return i;
        }
        return nums.length;
    }
}
```
Time complexity - O(n), worst case visits every element
Space complexity - O(1)

Sol 2: Optimal - Binary search tracking the insertion window
# Intuition
The array is sorted, so a comparison against the middle element tells us which half can never contain the insertion point. When the target is equal to `nums[mid]`, the answer is `mid`. When it's smaller, the insertion point (if any) lies to the left; when larger, to the right. When the loop ends without a hit, `low` has converged to exactly the first position where the target would fit — because `low` only advances past elements that are strictly smaller than the target.

1. Set `low = 0`, `high = n - 1`.
2. While `low <= high`, compute `guess = (low + high) / 2`.
3. If `nums[guess] == target`, return `guess`.
4. If `nums[guess] < target`, the insertion point is to the right — set `low = guess + 1`.
5. Otherwise the insertion point is to the left — set `high = guess - 1`.
6. When the loop ends, return `low` — the first index where the target can be placed.

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int n = nums.length;
        int low = 0;
        int high = n - 1;

        while (low <= high) {
            int guess = (low + high) / 2;
            if (nums[guess] == target) return guess;
            else if (nums[guess] < target) low = guess + 1;
            else high = guess - 1;
        }

        return low;
    }
}
```
Time complexity - O(log n), each step halves the remaining range
Space complexity - O(1)

## Key Takeaways
- "Find insertion index in sorted input" -> run binary search; when it exits with no match, `low` is the answer.
- `low` always lands on the first index whose value is >= target, which is exactly what keeps the array sorted.
- The `== target` early return is optional — skipping it would still leave `low` pointing at the target.
- Recognition cue: "sorted array" + "where would this go / first position that satisfies a condition" -> lower-bound binary search.