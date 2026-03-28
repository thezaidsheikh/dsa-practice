# Sliding Window Technique

## Definition
The sliding window technique involves maintaining a window of elements in an array or string that satisfies a specific condition, and dynamically adjusting the window size by sliding it across the data structure.

## When and Why to Use
- **When**: Problems involving subarrays/substrings with constraints (e.g., maximum/minimum sum, longest substring with unique characters, fixed-size window averages).
- **Why**: Reduces O(n²) brute-force solutions to O(n) by reusing computations from previous windows.

## How It Works
### Common Variations:
1. **Fixed-size window**: Window size remains constant (e.g., maximum sum of k consecutive elements).
2. **Variable-size window**: Window expands/shrinks based on conditions (e.g., longest substring with at most K distinct characters).

### Simple Example: Maximum Sum of k Consecutive Elements
**Problem**: Given an array of integers and an integer k, find the maximum sum of any k consecutive elements.
**Approach**:
- Initialize window sum with first k elements.
- Slide window one element at a time, subtracting the leftmost element and adding the new rightmost element.
- Track maximum sum encountered.

**Code Outline**:
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
- **Window Expansion/Contraction**: The key is determining when to expand (add new element) or contract (remove element) the window based on the condition.
- **Condition Checks**: Common conditions include:
  - Subarray sum equals target
  - Subarray contains at most K distinct characters
  - Subarray has all unique elements
- **Edge Cases**: Handle empty arrays, k=0, or k larger than array size.

## Differences from Other Patterns
- **vs. Two Pointers**: Sliding window maintains a fixed or variable window size, while two pointers often look for specific pairs/triplets without fixed boundaries.
- **vs. Prefix Sum**: Prefix sum is used for subarray sum queries, while sliding window dynamically adjusts the window to maintain a condition.
- **vs. Kadane's Algorithm**: Kadane's finds maximum subarray sum, while sliding window can be used for more complex conditions like distinct character counts.

## Template Questions to Remember When to Use
- Is the problem about finding a subarray/substring with specific properties?
- Do I need to maintain a window that satisfies a condition while sliding through the data?
- Can I reuse computations from previous windows to avoid recalculating from scratch?

## Type of Questions Solved
- **Subarray Problems**: Maximum/Minimum Sum, Longest Substring with K Distinct Characters, Subarray Product Less Than K
- **String Problems**: Longest Substring Without Repeating Characters, Minimum Window Substring
- **Array Problems**: Maximum Average Subarray, Subarray Sum Equals K

## Complexity
- **Time**: O(n) for single pass through the array/string.
- **Space**: O(1) extra space (not counting the input storage).

---
*Key Insight: The sliding window technique transforms problems with nested loops into linear passes by reusing computations from