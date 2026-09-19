# Overlapping Intervals

Prob: https://www.geeksforgeeks.org/problems/overlapping-intervals--174556/1

Sol 1: Brute Force - Compare every pair
1. Run two nested loops over all pairs of intervals (i, j) where i < j.
2. Two intervals overlap if intervals[i][1] >= intervals[j][0] and intervals[j][1] >= intervals[i][0].
3. If any pair overlaps, return true.

```java
class Solution {
    static boolean isIntersect(int[][] intervals) {
        int n = intervals.length;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (intervals[i][1] >= intervals[j][0] && intervals[j][1] >= intervals[i][0]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```
Time complexity - O(n^2),
Space complexity - O(1)

Sol 2: Better - Sort by start, track max end
1. Sort intervals by start time.
2. Keep track of the maximum end time seen so far.
3. If the current interval's start is <= max end, an overlap exists.

```java
class Solution {
    static boolean isIntersect(int[][] intervals) {
        int n = intervals.length;
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int maxEnd = intervals[0][1];
        for (int i = 1; i < n; i++) {
            if (intervals[i][0] <= maxEnd) return true;
            maxEnd = Math.max(maxEnd, intervals[i][1]);
        }
        return false;
    }
}
```
Time complexity - O(n log n),
Space complexity - O(1)

Sol 3: Optimal - Sort by start, check consecutive pairs
# Intuition
After sorting by start time, if any overlap exists, it must be between consecutive intervals in the sorted order. This is because if interval A ends before interval B starts, and B ends before C starts, then A also ends before C starts — no need to check non-adjacent pairs.

1. Sort intervals by start time.
2. Iterate through consecutive pairs.
3. If intervals[i][1] >= intervals[i+1][0], they overlap — return true.

```java
class Solution {
    static boolean isIntersect(int[][] intervals) {
        int n = intervals.length;
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        for (int i = 0; i < n - 1; i++) {
            if (intervals[i][1] >= intervals[i + 1][0]) return true;
        }
        return false;
    }
}
```
Time complexity - O(n log n),
Space complexity - O(1)
