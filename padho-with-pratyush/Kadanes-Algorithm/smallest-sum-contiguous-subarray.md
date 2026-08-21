Prob: https://www.geeksforgeeks.org/problems/smallest-sum-contiguous-subarray/1

Sol 1: Brute Force - Checking all subarrays
1. Run a loop (i) that selects every possible starting index of the subarray.
2. Run a loop (j) that signifies the ending index of the subarray.
3. Calculate the sum of each subarray and track the minimum sum seen so far.
4. Return the minimum sum.

```java
class Solution {
    public int minSubarraySum(int[] arr) {
        int n = arr.length;
        int min = Integer.MAX_VALUE;

        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int sum = 0;
                for (int m = i; m <= j; m++) {
                    sum += arr[m];
                }
                min = Math.min(min, sum);
            }
        }
        return min;
    }
}
```
Time Complexity: O(n^3), because for every pair of start and end indices we recompute the subarray sum from scratch.
Space Complexity: O(1)

Sol 2: Better - Building the sum incrementally
1. Notice that the sum of the subarray ending at j can be built incrementally from the subarray ending at j-1.
2. As j moves forward, add arr[j] to the running sum instead of recomputing from scratch.
3. Track the minimum running sum across all subarrays.

```java
class Solution {
    public int minSubarraySum(int[] arr) {
        int n = arr.length;
        int min = Integer.MAX_VALUE;

        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += arr[j];
                min = Math.min(min, sum);
            }
        }
        return min;
    }
}
```
Time Complexity: O(n^2), for each start index we extend the subarray while maintaining a running sum.
Space Complexity: O(1)

Sol 3: Optimal - Modified Kadane's algorithm (min version)
# Intuition
This is standard Kadane's algorithm flipped from max to min. For every index i, `best` holds the minimum sum of a subarray ending exactly at i. A subarray ending at i either starts fresh at arr[i], or extends the best subarray ending at i-1 by adding arr[i]. Extending only makes sense if it lowers the sum, so we take the smaller of the two choices. Since every element is visited once and each element alone forms a valid non-empty subarray, tracking the global minimum of all `best` values gives the answer even for all-negative arrays.

1. Initialize `best` to 0 and `min` to Integer.MAX_VALUE.
2. For each element, update `best = min(arr[i] + best, arr[i])`: either extend the previous subarray or start a new one at arr[i].
3. Update the global answer `min` with the current `best`.
4. Return `min`.

```java
class Solution {
    public int minSubarraySum(int[] arr) {
        int n = arr.length;
        int best = 0;
        int min = Integer.MAX_VALUE;

        for (int i = 0; i < n; i++) {
            best = Math.min(arr[i] + best, arr[i]);
            min = Math.min(min, best);
        }
        return min;
    }
}
```
Time Complexity: O(n), single pass over the array.
Space Complexity: O(1)

## Key Takeaways
- Smallest sum contiguous subarray = Kadane's algorithm with min instead of max at both decision points.
- Recognition cue: "minimum sum of a contiguous subarray" -> same DP state as Kadane (`best subarray ending here`), just minimizing.
- Alternative trick: negate every element, run standard max-Kadane, then negate the result.
