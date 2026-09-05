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

Sol 2: Optimal - Two Pointers
1. Use two pointers i and j for firstList and secondList.
2. At each step, check if the current intervals overlap.
3. Overlap exists when max(start1, start2) <= min(end1, end2).
4. If yes, add the intersection to the result.
5. Advance the pointer of the interval that ends first (it cannot overlap with anything further in the other list).

```java
class Solution {
    public int[][] intervalIntersection(int[][] firstList, int[][] secondList) {
        int i = 0;
        int j = 0;
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
