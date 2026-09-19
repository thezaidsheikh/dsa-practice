# Interval List Intersections

Prob: https://leetcode.com/problems/interval-list-intersections/description/

Sol 1: Brute Force - Compare every pair
1. For each interval in firstList, compare with every interval in secondList.
2. Check if they overlap: max(start1, start2) <= min(end1, end2).
3. If yes, add the intersection [max(start1, start2), min(end1, end2)] to the result.

```java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        List<int[]> ans = new ArrayList<>();
        for (int[] f : firstList) {
            for (int[] s : secondList) {
                int start = Math.max(f[0], s[0]);
                int end = Math.min(f[1], s[1]);
                if (start <= end) {
                    ans.add(new int[]{start, end});
                }
            }
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```
Time complexity - O(n * m),
Space complexity - O(n + m)

Sol 2: Better - Using binary search
1. For each interval in firstList, binary search in secondList to find the first interval that could overlap.
2. Then iterate forward from that point to collect all overlapping intervals.
3. This avoids comparing pairs that are clearly far apart.

```java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        List<int[]> ans = new ArrayList<>();
        for (int[] f : firstList) {
            int lo = 0, hi = secondList.length - 1;
            while (lo <= hi) {
                int mid = lo + (hi - lo) / 2;
                if (secondList[mid][1] < f[0]) lo = mid + 1;
                else hi = mid - 1;
            }
            for (int j = lo; j < secondList.length && secondList[j][0] <= f[1]; j++) {
                int start = Math.max(f[0], secondList[j][0]);
                int end = Math.min(f[1], secondList[j][1]);
                if (start <= end) {
                    ans.add(new int[]{start, end});
                }
            }
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```
Time complexity - O(n log m),
Space complexity - O(n + m)

Sol 3: Optimal - Two Pointers
# Intuition
Both lists are sorted. At any moment, the only intervals that can produce an intersection are the current pair pointed to by i and j. If firstList[i] ends before secondList[j], it cannot overlap with anything later in secondList, so advance i. Vice versa for j. This guarantees a single linear pass.

1. Use two pointers i and j for firstList and secondList.
2. At each step, compute the overlap: max(start1, start2) and min(end1, end2).
3. If max(start) <= min(end), the intervals overlap — add the intersection.
4. Advance the pointer of the interval that ends first (it cannot overlap with anything further in the other list).

```java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        int i = 0, j = 0;
        List<int[]> ans = new ArrayList<>();

        while (i < firstList.length && j < secondList.length) {
            int start = Math.max(firstList[i][0], secondList[j][0]);
            int end = Math.min(firstList[i][1], secondList[j][1]);

            if (start <= end) {
                ans.add(new int[]{start, end});
            }

            if (firstList[i][1] < secondList[j][1]) {
                i++;
            } else {
                j++;
            }
        }

        return ans.toArray(new int[ans.size()][]);
    }
}
```
Time complexity - O(n + m),
Space complexity - O(n + m)
