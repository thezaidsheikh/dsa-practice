# Ceil in a Sorted Array

Prob: https://www.geeksforgeeks.org/problems/ceil-in-a-sorted-array/1

## Problem Summary
Given a sorted array `arr` and an integer `x`, return the index of the smallest element that is greater than or equal to `x` (the ceil). If no such element exists, return -1.

Sol 1: Brute Force - Linear scan
1. Walk the array from left to right.
2. Return the first index whose value is greater than or equal to x.
3. If no element qualifies, return -1.

```java
class Solution {
    public int findCeil(int[] arr, int x) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] >= x) return i;
        }
        return -1;
    }
}
```
Time complexity - O(n), worst case checks every element
Space complexity - O(1)

Sol 2: Optimal - Binary search lowering the boundary
# Intuition
The array is sorted, so whenever we see a value that is already greater than or equal to x, that index is a candidate ceil — but a smaller index might qualify too, so we keep narrowing from the right. Whenever a value is smaller than x, the ceil can only be to the right. Each comparison discards half the range, and the last recorded candidate is the leftmost valid ceil.

1. Set `low = 0`, `high = n - 1`, and `min = -1` as the default "no ceil found".
2. While `low <= high`, compute `guess = (low + high) / 2`.
3. If `arr[guess] < x`, the ceil lies to the right — set `low = guess + 1`.
4. Else this index is a valid ceil candidate — save it as `min` and search the left half with `high = guess - 1`.
5. After the loop, return `min` (leftmost valid ceil) or -1 if none exists.

```java
class Solution {
    public int findCeil(int[] arr, int x) {
        int n = arr.length;
        int low = 0;
        int high = n - 1;
        int min = -1;

        while (low <= high) {
            int guess = (low + high) / 2;
            if (arr[guess] < x) {
                low = guess + 1;
            } else {
                min = guess;
                high = guess - 1;
            }
        }

        return min;
    }
}
```
Time complexity - O(log n), each step halves the remaining range
Space complexity - O(1)

## Key Takeaways
- "Find smallest index with value >= x" in sorted input -> binary search that keeps tightening `high` toward the leftmost valid candidate.
- Track the candidate inside the `else` branch and keep searching left — this is the standard "lower bound / ceil" variant.
- The default `min = -1` cleanly handles the case where every element is smaller than x.
- Recognition cue: "ceil / lower bound / smallest element >= target" -> same eliminate-half pattern as `arr[mid] >= x` search.