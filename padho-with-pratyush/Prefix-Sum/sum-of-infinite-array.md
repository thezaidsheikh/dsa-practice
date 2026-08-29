Prob: https://www.naukri.com/code360/problems/sum-of-infinite-array_873335

## Problem Summary
You are given an array `A` of size `n`. Define an infinite array `B` by repeating `A` forever: `B = A A A A ...`. For each query `[L, R]` (1-based), return the sum of `B[L..R]` modulo `10^9 + 7`. Constraints make a direct simulation impossible, so ranges must be answered with modular arithmetic.

Sol 1: Brute Force - Simulate each query
1. For each query, walk every index from `L` to `R`, reading the value from the repeated array (`A[(idx-1) % n]`) and summing it.
2. Take the result modulo `10^9 + 7`.
3. This is correct but far too slow: a single query can span up to `10^18` indices.

```java
import java.util.*;

public class Solution {
    static final long MOD = 1_000_000_007L;

    public static List<Integer> sumInRanges(int[] arr, int n, List<List<Long>> queries, int q) {
        List<Integer> ls = new ArrayList<>();
        for (List<Long> query : queries) {
            long L = query.get(0);
            long R = query.get(1);
            long sum = 0;
            for (long idx = L; idx <= R; idx++) {
                int val = arr[(int) ((idx - 1) % n)];
                sum = (sum + val) % MOD;
            }
            ls.add((int) sum);
        }
        return ls;
    }
}
```
Time Complexity: O(q * (R - L + 1)) — infeasible; a query can cover up to ~10^18 elements.
Space Complexity: O(q) for the output list (O(1) extra otherwise).

Sol 2: Better - Prefix of one copy + segmented range sum
# Intuition
Precompute `prefix[i]` = sum of `A[0..i-1]` mod MOD, and `total` = sum of one full copy. A range `[L, R]` can cross copy boundaries, so split it into at most three parts: a partial tail of the copy containing `L`, zero or more complete middle copies, and a partial head of the copy containing `R`. Each part is computed with the prefix array in O(1). This is O(1) per query but fiddly with the 1-based wrap-around math.

1. Build `prefix[0..n]` and `total = prefix[n]`.
2. Write a helper that returns the sum of `B[1..x]` by combining full copies and a remainder: `full = x / n`, `rem = x % n`, `sum = (full % MOD) * total + prefix[rem]`.
3. Answer each query as `sum(B[1..R]) - sum(B[1..L-1])` (mod MOD, keeping it non-negative).

The code below consolidates the wrap logic but keeps the three-part reasoning explicit per query instead of a single clean `f(x)` helper.

```java
import java.util.*;

public class Solution {
    static final long MOD = 1_000_000_007L;

    public static List<Integer> sumInRanges(int[] arr, int n, List<List<Long>> queries, int q) {
        long[] prefix = new long[n + 1];
        for (int i = 1; i <= n; i++) prefix[i] = (prefix[i - 1] + arr[i - 1]) % MOD;
        long total = prefix[n];

        List<Integer> ls = new ArrayList<>();
        for (List<Long> query : queries) {
            long L = query.get(0);
            long R = query.get(1);

            long sumR = rangeSum(R, n, total, prefix);
            long sumL = rangeSum(L - 1, n, total, prefix);
            long ans = (sumR - sumL + MOD) % MOD;
            ls.add((int) ans);
        }
        return ls;
    }

    // sum of B[1..x]; here done with explicit copy/remainder handling per call
    private static long rangeSum(long x, int n, long total, long[] prefix) {
        if (x <= 0) return 0;
        long fullCopies = x / n;
        int rem = (int) (x % n);
        long sum = ((fullCopies % MOD) * total) % MOD;
        sum = (sum + prefix[rem]) % MOD;
        return sum;
    }
}
```
Time Complexity: O(n + q), one prefix build plus O(1) work per query.
Space Complexity: O(n) for the prefix array.

Sol 3: Optimal - Prefix sum + closed-form `f(x)` helper
# Intuition
The key realization is that `sum(B[1..x])` has a clean closed form because the array repeats every `n` elements: take `fullCopies = x / n` whole copies (each contributing `total`) plus the first `x % n` elements of one copy (from the prefix array). Then any query is just `f(R) - f(L-1)` mod MOD — the subtraction cancels every element outside `[L, R]`. Working entirely in `long` with `% MOD` at each step prevents overflow. This is exactly the Better approach, refactored into a single reusable `f(x)` so the query loop is a one-liner.

1. Build `prefix[0..n]` and `total = prefix[n]`.
2. Define `f(x)`: if `x <= 0` return 0; else `fullCopies = x / n`, `remainder = x % n`, return `((fullCopies % MOD) * total + prefix[remainder]) % MOD`.
3. For each query, `ans = (f(R) - f(L - 1) + MOD) % MOD`.

```java
import java.util.*;

public class Solution {
    static final long MOD = 1_000_000_007L;

    public static List<Integer> sumInRanges(int[] arr, int n, List<List<Long>> queries, int q) {
        // prefix[i] = (arr[0] + ... + arr[i-1]) % MOD, with prefix[0] = 0
        long[] prefix = new long[n + 1];
        for (int i = 1; i <= n; i++) {
            prefix[i] = (prefix[i - 1] + arr[i - 1]) % MOD;
        }
        long total = prefix[n]; // sum of one full copy of A, mod MOD

        List<Integer> ls = new ArrayList<>();
        for (List<Long> query : queries) {
            long L = query.get(0);
            long R = query.get(1);
            // f(x) = sum of B[1..x] under mod
            long ans = (f(R, n, total, prefix) - f(L - 1, n, total, prefix) + MOD) % MOD;
            ls.add((int) ans);
        }
        return ls;
    }

    // sum of B[1..x] mod MOD, using 1-based infinite array
    private static long f(long x, int n, long total, long[] prefix) {
        if (x <= 0) return 0;
        long fullCopies = x / n;                 // number of complete copies of A
        int remainder   = (int) (x % n);         // leftover elements from a fresh copy
        long sum = (fullCopies % MOD) * total % MOD;   // full copies contribution
        sum = (sum + prefix[remainder]) % MOD;         // partial copy contribution
        return sum;
    }
}
```
Time Complexity: O(n + q), prefix build plus O(1) per query.
Space Complexity: O(n) for the prefix array.

## Key Takeaways
- Repeating-array range sums reduce to: `sum(B[1..x]) = (x/n) * total + prefix[x%n]`. A query is `f(R) - f(L-1)`.
- The `(x / n)` and `x % n` split is the heart of every "infinite/repeating array" range problem.
- Keep everything modulo `10^9 + 7` at each addition/multiplication, and use `long` to avoid 32-bit overflow. Add `MOD` before the final `%` so a negative difference becomes non-negative.
- Subtraction-trick recall: `f(R) - f(L-1)` cancels everything outside the query, just like prefix-sum difference on a normal array.
- Recognition cue: "sum of a repeating/infinite array over [L, R]" -> prefix of one copy + closed-form copy/remainder split, never simulate.
