Prob: https://www.naukri.com/code360/problems/longest-subarray-with-sum-k_6682399

Sol 1: Brute Force - Checking all subarrays
1. Run a loop (i) to pick the starting index of every subarray.
2. Run a loop (j) from i onward, building the sum incrementally.
3. Whenever the sum equals k, update the maximum length j - i + 1.
4. Return the maximum length found.
```java
public class Solution {
    public static int longestSubarrayWithSumK(int[] a, long k) {
        int n = a.length;
        int max = 0;

        for (int i = 0; i < n; i++) {
            long sum = 0;
            for (int j = i; j < n; j++) {
                sum += a[j];
                if (sum == k) {
                    max = Math.max(max, j - i + 1);
                }
            }
        }

        return max;
    }
}
```
Time Complexity: O(n^2), every start-end pair is examined.
Space Complexity: O(1)

Sol 2: Better - Prefix sum + HashMap
1. Build a running prefix sum as you traverse the array.
2. At each index, check if (prefixSum - k) has been seen before. If yes, the subarray between that previous index and current index sums to k.
3. Store the first occurrence of each prefix sum (only store if not already present to get the longest subarray).
4. Update maxLen accordingly.
```java
import java.util.HashMap;

public class Solution {
    public static int longestSubarrayWithSumK(int[] a, long k) {
        int n = a.length;
        HashMap<Long, Integer> prefixMap = new HashMap<>();
        long sum = 0;
        int max = 0;

        for (int i = 0; i < n; i++) {
            sum += a[i];
            if (sum == k) {
                max = i + 1;
            }
            if (prefixMap.containsKey(sum - k)) {
                max = Math.max(max, i - prefixMap.get(sum - k));
            }
            if (!prefixMap.containsKey(sum)) {
                prefixMap.put(sum, i);
            }
        }

        return max;
    }
}
```
Time Complexity: O(n), single pass with HashMap lookups in O(1).
Space Complexity: O(n), for the prefix sum HashMap.

Sol 3: Optimal - Sliding window (variable size, works when all elements are positive)
# Intuition
Since all elements are positive, growing the window always increases the sum and shrinking it always decreases the sum. This monotonicity lets us expand the right pointer until the sum exceeds k, then shrink from the left until the sum is back within bounds. If the sum equals k at any point, we record the window length.

1. Expand the window by moving the right pointer and adding a[right] to the running sum.
2. While the sum exceeds k, shrink from the left by subtracting a[left] and moving left forward.
3. If the sum equals k, update the maximum length.
4. Move right forward and repeat.
```java
public class Solution {
    public static int longestSubarrayWithSumK(int[] a, long k) {
        int left = 0, right = 0;
        long sum = 0;
        int max = 0;
        int n = a.length;

        while (right < n) {
            sum += a[right];
            while (left <= right && sum > k) {
                sum -= a[left];
                left++;
            }
            if (sum == k) max = Math.max(max, right - left + 1);
            right++;
        }
        return max;
    }
}
```
Time Complexity: O(n), the left and right pointers each traverse the array once.
Space Complexity: O(1)

## Key Takeaways
- When all elements are positive, variable-size sliding window works because the sum is monotonic with window size.
- When elements can be negative, use prefix sum + HashMap instead.
- Recognition cue: "longest subarray with sum equal to k" + positive numbers -> sliding window. With negatives -> prefix sum + HashMap.
