# Rotation (Find Number of Rotations in Sorted Array)

Prob: https://www.geeksforgeeks.org/problems/rotation4723/1

## Problem Summary
Given a sorted array that has been rotated (left or right) some number of times, return the number of rotations. Equivalently, this is the index of the minimum element: rotating a sorted array moves the minimum to that index.

Sol 1: Brute Force - Linear scan for the rotation point
1. Walk the array and find the index where the next element is smaller than the current one — that is the drop point.
2. That index is the number of rotations; if no drop exists, the array is unrotated and the answer is 0.

```java
class Solution {
    public int findKRotation(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            if (arr[i] > arr[i + 1]) return i + 1;
        }
        return 0;
    }
}
```
Time complexity - O(n), scans until the drop point
Space complexity - O(1)

Sol 2: Optimal - Binary search on the last element
# Intuition
The array consists of a rotated-away prefix whose values are all larger than the last element, followed by a fully sorted suffix whose values are all smaller than or equal to it. So comparing `arr[mid]` with `arr[n - 1]` tells us which side holds the rotation boundary: if the middle is larger than the last element, we are still in the prefix and the boundary lies to the right (the min starts after `mid`); otherwise the middle is already in the sorted suffix, and the boundary lies at or to the left of `mid`. Each comparison halves the range, and the answer (index of the minimum) is exactly the number of rotations.

1. Set `low = 0`, `high = n - 1`, and `res = 0` as the default (unrotated).
2. While `low <= high`, compute `mid = (low + high) / 2`.
3. If `arr[mid] <= arr[n - 1]`, we are in the sorted suffix — search left with `high = mid - 1`.
4. Otherwise `mid` is in the rotated-away prefix — record `res = mid + 1` as the current best boundary and search right with `low = mid + 1`.
5. Return `res`, the index of the minimum (the rotation count).

```java
class Solution {
    public int findKRotation(int[] arr) {
        int n = arr.length;
        int low = 0;
        int high = n - 1;
        int res = 0;

        while (low <= high) {
            int mid = (low + high) / 2;
            if (arr[mid] <= arr[n - 1]) {
                high = mid - 1;
            } else {
                res = mid + 1;
                low = mid + 1;
            }
        }
        return res;
    }
}
```
Time complexity - O(log n), each step halves the remaining range
Space complexity - O(1)

## Key Takeaways
- "Number of rotations" is the same ask as "index of the minimum" in a rotated sorted array.
- The last element is the pivot reference: `arr[mid] > arr[last]` means we are left of the boundary and must move right.
- Unrotated input (or a full rotation) leaves `res = 0`, because every middle value is <= the last element.
- Recognition cue: "find rotation count / minimum in a rotated array" -> compare the middle against an endpoint.