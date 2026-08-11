Prob: https://www.geeksforgeeks.org/problems/max-sum-subarray-of-size-k5313/1

Sol 1: Brute Force - Checking all subarrays of size k
1. Run a loop (i) from 0 to n-k to pick the starting index of every subarray of size k.
2. For each start, run an inner loop to compute the sum of the k elements.
3. Track the maximum sum seen so far and return it.
```java
class Solution {
    public int maxSubarraySum(int[] arr, int k) {
        int n = arr.length;
        int max = Integer.MIN_VALUE;

        for (int i = 0; i <= n - k; i++) {
            int sum = 0;
            for (int j = i; j < i + k; j++) {
                sum += arr[j];
            }
            max = Math.max(sum, max);
        }

        return max;
    }
}
```
Time Complexity: O(n*k), because for each start index we recompute the sum of k elements.
Space Complexity: O(1)

Sol 2: Better - Using prefix sum
1. Build a prefix sum array where prefix[i] = sum of elements from index 0 to i-1.
2. Sum of a subarray of size k starting at i = prefix[i+k] - prefix[i].
3. Compute this in O(1) per start index using the prefix array.
```java
class Solution {
    public int maxSubarraySum(int[] arr, int k) {
        int n = arr.length;
        int[] prefix = new int[n + 1];

        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + arr[i];
        }

        int max = Integer.MIN_VALUE;
        for (int i = 0; i <= n - k; i++) {
            int sum = prefix[i + k] - prefix[i];
            max = Math.max(sum, max);
        }

        return max;
    }
}
```
Time Complexity: O(n), since we build the prefix array in one pass and query each window in O(1).
Space Complexity: O(n), for the prefix array.

Sol 3: Optimal - Sliding window (fixed size)
# Intuition
Adjacent windows of size k share k-1 elements. When we slide the window from [i, i+k-1] to [i+1, i+k], the sum changes by only two elements: we drop arr[i] and add arr[i+k-1]. Instead of recomputing the whole window, reuse the previous sum.

1. Compute the sum of the first window (first k elements) and treat it as the initial max.
2. Slide the window one step at a time: subtract the element leaving the window and add the element entering it.
3. Update max after every slide.
4. Return the maximum sum.
```java
class Solution {
    public int maxSubarraySum(int[] arr, int k) {
        int n = arr.length;
        int max = 0;
        int sum = 0;

        for (int i = 0; i < k; i++) {
            sum += arr[i];
        }
        max = sum;

        for (int i = k; i < n; i++) {
            sum += arr[i] - arr[i - k];
            max = Math.max(sum, max);
        }

        return max;
    }
}
```
Time Complexity: O(n), each element is added once and removed once.
Space Complexity: O(1)

## Key Takeaways
- This is the classic fixed-size sliding window problem.
- Recognition cue: "sum of every subarray of size k" -> slide a window of fixed size and maintain the sum incrementally.
