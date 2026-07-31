# Sliding Window Technique

## Definition
The sliding window technique involves maintaining a window of elements in an array or string that satisfies a specific condition, and dynamically adjusting the window size by sliding it across the data structure. The window is defined by two pointers (or indices) that both move forward, so redundant recomputation is avoided while efficiently finding subarrays/substrings that satisfy the condition.

## When and Why to Use
- **When**: Problems involving subarrays/substrings with constraints — maximum/minimum sum, longest substring with unique characters, fixed-size window averages. General signal: the problem asks for the maximum/minimum/longest/shortest subarray or substring with a specific property over contiguous elements.
- **Why**: Reduces O(n²) brute-force solutions to O(n) by reusing computations from previous windows. Every element enters and exits the window at most once.

## Why It Works
Instead of recalculating from scratch for each position, we maintain state from the previous window and adjust it incrementally (add one element on the right, remove one on the left).

## How It Works
### Fixed-Size Window
Window size is constant (k). Move the window one step at a time, updating the result in O(1).

```java
int windowSum = 0;
for (int i = 0; i < k; i++) {
    windowSum += arr[i];
}
for (int i = k; i < n; i++) {
    windowSum += arr[i] - arr[i - k]; // slide
    // evaluate window
}
```

#### Example: Maximum Sum Subarray of Size K
```java
public int maxSum(int[] arr, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    int maxSum = windowSum;
    for (int i = k; i < arr.length; i++) {
        windowSum += arr[i] - arr[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

### Variable-Size Window
Window size expands/shrinks based on a condition. Common for "at most", "at least", or "longest/shortest" problems.

```java
int left = 0;
for (int right = 0; right < n; right++) {
    // expand window by adding arr[right]
    while (conditionViolated) {
        // shrink window from left
        left++;
    }
    // evaluate window
}
```

#### Example: Longest Substring with At Most K Distinct Characters
```java
public int longestSubstringKDistinct(String s, int k) {
    HashMap<Character, Integer> map = new HashMap<>();
    int left = 0, maxLen = 0;
    for (int right = 0; right < s.length(); right++) {
        map.put(s.charAt(right), map.getOrDefault(s.charAt(right), 0) + 1);
        while (map.size() > k) {
            char leftChar = s.charAt(left);
            map.put(leftChar, map.get(leftChar) - 1);
            if (map.get(leftChar) == 0) map.remove(leftChar);
            left++;
        }
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

#### Example: Minimum Size Subarray Sum (At Least Target)
```java
public int minSubarrayLen(int target, int[] nums) {
    int left = 0, sum = 0, minLen = Integer.MAX_VALUE;
    for (int right = 0; right < nums.length; right++) {
        sum += nums[right];
        while (sum >= target) {
            minLen = Math.min(minLen, right - left + 1);
            sum -= nums[left++];
        }
    }
    return minLen == Integer.MAX_VALUE ? 0 : minLen;
}
```

### Simple Example Walkthrough: Maximum Sum of k Consecutive Elements
**Problem**: Given an array of integers and an integer k, find the maximum sum of any k consecutive elements.
**Approach**:
- Initialize window sum with the first k elements.
- Slide the window one element at a time, subtracting the leftmost element and adding the new rightmost element.
- Track the maximum sum encountered.

**Code Outline (Python)**:
```python
def max_sum_k(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum
    for i in range(k, len(arr)):
        window_sum = window_sum - arr[i-k] + arr[i]
        max_sum = max(max_sum, window_sum)
    return max_sum
```

## Important Insights and Definitions
- **Window Expansion/Contraction**: The key is determining when to expand (add a new element) or contract (remove an element) based on the condition.
- **Condition Checks**: Common conditions include:
  - Subarray sum equals target
  - Subarray contains at most K distinct characters
  - Subarray has all unique elements
- **Edge Cases**: Handle empty arrays, k=0, or k larger than the array size.

### Common Question Types
| Type | Condition | Example |
|------|-----------|---------|
| Maximum sum (fixed k) | Sum of k elements | Max sum subarray of size k |
| Longest substring | At most K distinct | Longest with ≤ 2 distinct chars |
| Shortest subarray | At least sum S | Min length ≥ target sum |
| Longest repeating | All unique | Longest without repeating chars |
| Window contains | Target chars | Minimum window substring |

## Complexity Analysis
- **Time**: O(n) — each element enters and exits the window at most once.
- **Space**: O(1) for fixed-size; O(min(n, charset)) for variable-size with a hash map.

## Edge Cases to Watch For
- Empty array/string.
- k = 0 or k larger than the array size.
- Negative numbers: variable-size sum windows require non-negative numbers to keep the sum monotonic; with negatives use prefix sum + hash map.
- Window never satisfies the condition (e.g., total sum < target) → return 0.
- Track the right quantity for the condition (count vs. distinct count vs. sum).

## Differences from Other Patterns
- **vs. Two Pointers**: Sliding window maintains a fixed or variable window size; two pointers often look for specific pairs/triplets without fixed boundaries. Sliding window has a bounded window; two pointers find pairs without a fixed size.
- **vs. Prefix Sum**: Prefix sum is used for subarray sum queries, while sliding window dynamically adjusts the window to maintain a condition. Prefix sum answers queries; sliding window maintains a dynamic constraint.
- **vs. Kadane's Algorithm**: Kadane's finds the maximum subarray sum, while sliding window handles more complex conditions like distinct character counts. Kadane's handles negative numbers optimally; sliding window requires a constraint to maintain.

## Recognition Cues / Template Questions
- Is the problem about finding a subarray/substring with specific properties?
- Do I need to maintain a window that satisfies a condition while sliding through the data?
- Can I reuse computations from previous windows to avoid recalculating from scratch?
- "Find the longest/shortest subarray/substring with..."
- "Maximum sum of k consecutive elements"
- "Contains at most/at least K..."
- "Subarray sum equals/at least..."

## Type of Questions Solved
- **Subarray Problems**: Maximum/Minimum Sum, Longest Substring with K Distinct Characters, Subarray Product Less Than K
- **String Problems**: Longest Substring Without Repeating Characters, Minimum Window Substring
- **Array Problems**: Maximum Average Subarray, Subarray Sum Equals K

## Limitations / When Not to Use
- **Negative numbers in sum-constrained windows**: Expanding can decrease the sum, so monotonicity breaks. Use prefix sum + hash map instead.
- **No clear shrink rule**: If the condition doesn't tell you when to shrink the window, the pattern doesn't apply.
- **Fixed vs. variable confusion**: Pick fixed when k is given; pick variable when you must find the best window satisfying a condition.
- **Space cost**: Variable-size windows using a hash map take O(min(n, charset)) space, so two pointers or prefix sum may be better when O(1) space matters.

## Key Takeaways
- Sliding window turns nested loops into a single pass by maintaining state. Use **fixed** when the size is given; use **variable** when you need to find the best window that satisfies a condition.
- The technique works because every element enters and exits the window at most once.
