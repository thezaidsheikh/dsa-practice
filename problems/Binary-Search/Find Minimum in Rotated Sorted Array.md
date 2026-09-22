# Find Minimum in Rotated Sorted Array

Prob: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/

## Problem Summary
Given a sorted array that was rotated between 1 and n times, return the minimum element. The array contains distinct values. Must run in O(log n).

Sol 1: Brute Force - Linear scan
1. Initialize the minimum with the first element.
2. Walk the whole array and keep the smallest value seen.
3. Return the minimum.

```java
class Solution {
    public int findMin(int[] nums) {
        int min = nums[0];
        for (int i = 1; i < nums.length; i++) {
            min = Math.min(min, nums[i]);
        }
        return min;
    }
}
```
Time complexity - O(n), visits every element
Space complexity - O(1)

Sol 2: Optimal - Binary search against the last element
# Intuition
In a rotated sorted array the last element is a fixed reference point: the minimum is the first element that is smaller than or equal to `nums[n - 1]`. Every element to the left of that boundary is larger than the last element, and every element from the boundary onward is smaller. So comparing `nums[mid]` with `nums[n - 1]` tells us exactly which half contains the minimum — if the middle is bigger than the last element we are still in the rotated-away prefix and must go right; otherwise the middle itself is a candidate and the minimum can only be to its left.

1. Set `low = 0`, `high = n - 1`, and `lowest = 0` as the default candidate.
2. While `low <= high`, compute `mid = (low + high) / 2`.
3. If `nums[mid] > nums[n - 1]`, the boundary is to the right — set `low = mid + 1`.
4. Otherwise `nums[mid]` is inside the sorted run that contains the minimum: record `lowest = nums[mid]` and tighten `high = mid - 1` to keep looking left.
5. Return `lowest`, the smallest candidate found.

```java
class Solution {
    public int findMin(int[] nums) {
        int n = nums.length;
        int low = 0;
        int high = n - 1;
        int lowest = 0;

        while (low <= high) {
            int mid = (low + high) / 2;
            if (nums[mid] > nums[n - 1]) {
                low = mid + 1;
            } else {
                lowest = nums[mid];
                high = high - 1;
            }
        }
        return lowest;
    }
}
```
Time complexity - O(log n), each step halves the remaining range
Space complexity - O(1)

## Key Takeaways
- The last element (or first element) of a rotated sorted array is the pivot reference: `nums[mid] > nums[last]` means we are left of the rotation and must search right.
- Every candidate found in the else branch is monotonically decreasing, so the last one stored is the minimum.
- Recognition cue: "sorted array that was rotated" + "find minimum / maximum" -> compare the middle against an endpoint to decide which half holds the boundary.