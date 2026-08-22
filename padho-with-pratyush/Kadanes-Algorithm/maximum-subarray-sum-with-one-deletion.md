Prob: https://leetcode.com/problems/maximum-subarray-sum-with-one-deletion/

Sol 1: Brute Force - Try deleting every element, run Kadane on the rest
1. For each possible deleted index (and the "delete nothing" option), run standard Kadane's algorithm on the array while skipping that index.
2. Track the maximum subarray sum across all these runs.
3. Return the overall maximum.

```java
class Solution {
    public int maximumSum(int[] arr) {
        int n = arr.length;
        int ans = Integer.MIN_VALUE;

        for (int del = -1; del < n; del++) {   // del = -1 means no deletion
            int best = Integer.MIN_VALUE;
            int cur = 0;
            for (int i = 0; i < n; i++) {
                if (i == del) continue;        // simulate the deletion
                cur = Math.max(arr[i], cur + arr[i]);
                best = Math.max(best, cur);
            }
            ans = Math.max(ans, best);
        }
        return ans;
    }
}
```
Time Complexity: O(n^2), one Kadane pass per deletion candidate.
Space Complexity: O(1)

Sol 2: Better - Prefix and suffix Kadane arrays
# Intuition
Deleting arr[i] only matters if the optimal subarray wraps around it: the part ending just before i joined with the part starting just after i. Precompute `pre[i]` = max subarray sum ending at i (forward Kadane) and `suf[i]` = max subarray sum starting at i (backward Kadane). Then deleting arr[i] gives the candidate pre[i-1] + suf[i+1], and the no-deletion answer is simply max(pre).

1. Fill `pre` with a forward Kadane pass and `suf` with a backward pass.
2. The no-deletion answer is max over all pre[i].
3. For each interior index i (1 to n-2), consider deleting arr[i]: candidate = pre[i-1] + suf[i+1].
4. Boundary deletions (first or last element) never beat the no-deletion maximum, since deletion is optional — no special handling needed.

```java
class Solution {
    public int maximumSum(int[] arr) {
        int n = arr.length;
        int[] pre = new int[n];
        int[] suf = new int[n];

        pre[0] = arr[0];
        for (int i = 1; i < n; i++) {
            pre[i] = Math.max(arr[i], pre[i - 1] + arr[i]);
        }

        suf[n - 1] = arr[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            suf[i] = Math.max(arr[i], suf[i + 1] + arr[i]);
        }

        int res = Integer.MIN_VALUE;
        for (int i = 0; i < n; i++) {          // no deletion needed
            res = Math.max(res, pre[i]);
        }
        for (int i = 1; i < n - 1; i++) {      // delete arr[i]
            res = Math.max(res, pre[i - 1] + suf[i + 1]);
        }
        return res;
    }
}
```
Time Complexity: O(n), three linear passes.
Space Complexity: O(n), for the two auxiliary arrays.

Sol 3: Optimal - Two-state rolling DP (Kadane with a "deleted" state)
# Intuition
Carry Kadane's DP forward with two states instead of one: `max_without_del` = best subarray sum ending here having used NO deletion, and `max_with_del` = best subarray sum ending here having used exactly ONE deletion. Each new element has clear transitions: without deletion it is standard Kadane (extend or restart). With deletion there are two ways to arrive — extend an already-deleted subarray by arr[i], or spend the deletion on arr[i] itself, which leaves intact the best subarray ending at the previous index. Because both states only depend on the previous values, we roll them in O(1) space. The MIN_VALUE sentinel marks "no valid deleted state yet", and the global answer tracks the max of both states.

1. Initialize max_without_del = arr[0], max_with_del = Integer.MIN_VALUE, max = arr[0].
2. Save prev_max_without_del and prev_max_with_del before updating.
3. Update max_without_del = max(arr[i], prev_max_without_del + arr[i]).
4. Compute v2: if the previous deleted state is invalid (MIN_VALUE), seed it with arr[i]; otherwise v2 = prev_max_with_del + arr[i].
5. Update max_with_del = max(v2, prev_max_without_del) — extend an already-deleted subarray, or delete arr[i].
6. Track max as the largest of both current states; return max.

```java
class Solution {
    public int maximumSum(int[] arr) {
        int max = arr[0];
        int max_without_del = arr[0];
        int max_with_del = Integer.MIN_VALUE;

        for(int i = 1; i < arr.length; i++) {
            int prev_max_without_del = max_without_del;
            int prev_max_with_del = max_with_del;
            max_without_del = Math.max(arr[i],max_without_del+arr[i]);

            int v2;
            if(prev_max_with_del == Integer.MIN_VALUE) {
                v2 = arr[i];
            } else {
                v2 = prev_max_with_del + arr[i];
            }
            max_with_del = Math.max(v2,prev_max_without_del);
            max = Math.max(max,Math.max(max_with_del,max_without_del));
        }
        return max;
    }
}
```
Time Complexity: O(n), single pass over the array.
Space Complexity: O(1)

## Key Takeaways
- "One deletion allowed" turns Kadane into a two-state DP: best ending here WITHOUT deletion and best ending here WITH one deletion.
- The key transition is max_with_del = max(max_with_del_prev + arr[i], max_without_del_prev) — either the deletion already happened earlier, or arr[i] IS the deleted element.
- Same idea generalizes to k deletions by carrying k+1 states.
- Recognition cue: "subarray problem with one removal/one skip allowed" -> add a parallel DP state rather than restarting brute force.
- Sibling of Maximum Subarray in this folder — identical skeleton, upgraded from one rolling state to two.
