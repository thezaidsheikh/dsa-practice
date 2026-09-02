Prob: https://leetcode.com/problems/merge-intervals/description/

## Problem Summary
Given an array of `intervals` where `intervals[i] = [start_i, end_i]`, merge all overlapping intervals and return an array of the non-overlapping intervals that cover all the input.

Sol 1: Brute Force - Repeatedly scan and merge
1. Sort the intervals by start time.
2. Repeatedly scan the list; for any two consecutive intervals that overlap (i.e. `current.start <= next.end`), merge them into a single interval by extending the end to `max(current.end, next.end)`.
3. Keep scanning from the beginning after each merge until no more merges happen.

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        int n = intervals.length;
        boolean merged = true;

        while (merged) {
            merged = false;
            for (int i = 0; i < n - 1; i++) {
                if (intervals[i][0] <= intervals[i + 1][0]
                        && intervals[i][1] >= intervals[i + 1][0]) {
                    intervals[i][1] = Math.max(intervals[i][1], intervals[i + 1][1]);
                    for (int j = i + 1; j < n - 1; j++) {
                        intervals[j] = intervals[j + 1];
                    }
                    n--;
                    merged = true;
                    break;          // restart scan from the beginning
                }
            }
        }

        int[][] result = new int[n][2];
        System.arraycopy(intervals, 0, result, 0, n);
        return result;
    }
}
```
Time Complexity: O(n²), each pass is O(n) and there can be O(n) passes in the worst case.
Space Complexity: O(1) auxiliary (shifting done in-place).

Sol 2: Better - Sort + merge on a result list
# Intuition
After sorting by start, overlapping intervals are guaranteed to be neighbours, so a single forward scan suffices: keep appending to the result list when intervals overlap, and start a new entry when they don't. This is logically correct but uses an `ArrayList` with explicit `.get()` access, which is slightly less compact than tracking a running reference.

1. Sort the intervals by start.
2. Maintain a result list, starting with the first interval.
3. For each subsequent interval, if it overlaps the last recorded interval, extend that interval's end; otherwise append it to the result list.

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length < 2) return intervals;
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        List<int[]> merged = new ArrayList<>();
        merged.add(intervals[0]);

        for (int i = 1; i < intervals.length; i++) {
            int[] last = merged.get(merged.size() - 1);
            if (intervals[i][0] <= last[1]) {
                last[1] = Math.max(last[1], intervals[i][1]);
            } else {
                merged.add(intervals[i]);
            }
        }

        return merged.toArray(new int[merged.size()][]);
    }
}
```
Time Complexity: O(n log n), dominated by sorting; the linear scan is O(n).
Space Complexity: O(n), for the output list (O(1) auxiliary otherwise).

Sol 3: Optimal - Sort + linear scan with in-place tracking
# Intuition
The logic is identical to Better, but written compactly using a single running reference (`finalInterval`) instead of an `ArrayList` — the result is built into a list only when the current interval cannot extend further. This is the cleanest implementation: sort first so overlapping intervals become neighbours, then walk once, extending or appending.

1. Sort the intervals by start.
2. Track `finalInterval = intervals[0]`. For each subsequent interval:
   - If it overlaps (`current.start <= finalInterval.end`), extend `finalInterval.end`.
   - Otherwise, push `finalInterval` into the result list and set `finalInterval = current`.
3. After the loop, push the last `finalInterval` and convert the list to an array.

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length < 2) return intervals;
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        int[] finalInterval = intervals[0];
        List<int[]> ls = new ArrayList<>();

        for (int i = 1; i < intervals.length; i++) {
            int[] currentInterval = intervals[i];
            if (currentInterval[0] <= finalInterval[1]) {
                finalInterval[1] = Math.max(currentInterval[1], finalInterval[1]);
            } else {
                ls.add(finalInterval);
                finalInterval = currentInterval;
            }
        }

        ls.add(finalInterval);
        return ls.toArray(new int[ls.size()][]);
    }
}
```
Time Complexity: O(n log n), sorting dominates; single linear scan after.
Space Complexity: O(n), for the output list (O(1) auxiliary).

## Key Takeaways
- The core insight is that after sorting by start, overlapping intervals are guaranteed to be neighbours — so a single forward scan suffices.
- Merge rule: if the next interval's start ≤ the current interval's end, extend; otherwise push the current and start fresh.
- `Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]))` is the standard interval sort; never sort by end.
- The in-place reference-tracking version (Sol 3) is the interview cleanest — it avoids an `ArrayList` while remaining O(n) auxiliary for the output.
- Recognition cue: "merge / overlap / combine intervals" -> sort by start, then single linear scan.
