# Koko Eating Bananas

Prob: https://leetcode.com/problems/koko-eating-bananas/description/

## Problem Summary
Koko must eat all `piles` of bananas within `h` hours. She can choose one eating speed (bananas per hour); if a pile has fewer bananas than her speed, she finishes it in one hour and moves on. Find the minimum speed that lets her finish in time.

Sol 1: Brute Force - Try every speed from 1 to max
1. The answer lies between 1 and the largest pile (no speed faster than the biggest pile is ever needed).
2. For each speed, simulate the hours needed across all piles.
3. Return the first speed whose total hours is <= h.

```java
class Solution {
    public long calculateHours(int[] piles, int speed) {
        long h = 0;
        for (int pile : piles) {
            h += pile / speed;
            if (pile % speed != 0) h++;
        }
        return h;
    }

    public int minEatingSpeed(int[] piles, int h) {
        int max = 0;
        for (int pile : piles) max = Math.max(max, pile);

        for (int speed = 1; speed <= max; speed++) {
            if (calculateHours(piles, speed) <= h) return speed;
        }
        return max;
    }
}
```
Time complexity - O(max(pile) * n), every speed up to the largest pile is simulated
Space complexity - O(1)

Sol 2: Optimal - Binary search on the speed
# Intuition
Hours needed decreases as speed increases, so the condition "can finish within h" is monotonic: once a speed works, every larger speed works too. That monotonic search space (from 1 to the largest pile) can be halved at each step. Before guessing, find `high` as the largest pile by a single scan — no sorting needed. `calculateHours` uses a `long` accumulator because pile sums can overflow an `int` for large inputs (e.g., LeetCode's constraint of up to 10^9 bananas per pile).

1. Find `high` as the maximum pile with a single pass (`low = 1`).
2. While `low <= high`, compute `guess = (low + high) / 2`.
3. If `calculateHours(piles, guess) > h`, this speed is too slow — search the right half with `low = guess + 1`.
4. Otherwise the speed works: save it as `ans` and keep looking for a smaller valid speed with `high = guess - 1`.
5. Return `ans`, the smallest speed that finishes within h hours.

```java
class Solution {
    public long calculateHours(int[] piles, int speed) {
        long h = 0;
        for (int i = 0; i < piles.length; i++) {
            h += piles[i] / speed;
            if (piles[i] % speed != 0) h++;
        }
        return h;
    }

    public int minEatingSpeed(int[] piles, int h) {
        int n = piles.length;
        int low = 1;
        int high = Integer.MIN_VALUE;
        int ans = 0;

        for (int i = 0; i < n; i++) {
            if (piles[i] > high) high = piles[i];
        }

        while (low <= high) {
            int guess = (low + high) / 2;
            if (calculateHours(piles, guess) > h) {
                low = guess + 1;
            } else {
                ans = guess;
                high = guess - 1;
            }
        }
        return ans;
    }
}
```
Time complexity - O(n log m), where m is the largest pile; each check costs O(n) and there are O(log m) guesses
Space complexity - O(1)

## Key Takeaways
- Monotonic feasibility ("speed works => any larger speed works") -> binary search the answer instead of scanning it.
- Use the largest pile (not a sort) as the upper bound of the search range; sorting is unnecessary here.
- When summing hours over large piles, accumulate into a `long` to avoid integer overflow.
- Recognition cue: "minimum X such that a condition holds" with a monotonically increasing/decreasing condition -> answer-space binary search.