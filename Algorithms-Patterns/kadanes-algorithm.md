# Kadane's Algorithm

## Definition
Kadane's Algorithm is an efficient method for finding the **maximum sum of a contiguous subarray** (also known as the Maximum Subarray Problem) in a one-dimensional array of integers. It uses dynamic programming principles to solve the problem in O(n) time and O(1) space.

## When and Why to Use
- **When**: You need to find a contiguous subarray with the largest possible sum from a given array (which may contain negative numbers).
- **Why**: It's the optimal solution — faster than O(n²) brute-force and works even with negative numbers. It's also the foundation for many other variations (e.g., maximum product subarray, circular array variants).

## How It Works
The algorithm maintains two variables while iterating through the array:
- `current_max`: The maximum sum of a subarray ending at the current position.
- `global_max`: The maximum sum found so far across all positions.

For each element, you decide whether to:
1. Extend the previous subarray (add the current element to `current_max`)
2. Start a new subarray at the current element (`current_max = current_element`)

If `current_max` becomes negative, you reset it to the current element because a negative sum will reduce future subarray sums.

### Simple Example
**Problem**: Find the maximum sum of a contiguous subarray in `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`.
**Solution**:
- Start: `current_max = global_max = -2`
- i=1: element=1 → `current_max = max(1, -2+1)=1`, `global_max=1`
- i=2: element=-3 → `current_max = max(-3, 1-3)= -2`, `global_max=1` (not updated)
- i=3: element=4 → `current_max = max(4, -2+4)=4`, `global_max=4`
- i=4: element=-1 → `current_max = max(-1, 4-1)=3`, `global_max=4`
- i=5: element=2 → `current_max = max(2, 3+2)=5`, `global_max=5`
- i=6: element=1 → `current_max = max(1, 5+1)=6`, `global_max=6`
- i=7: element=-5 → `current_max = max(-5, 6-5)=1`, `global_max=6`
- i=8: element=4 → `current_max = max(4, 1+4)=5`, `global_max=6`
- Result: `global_max = 6` (subarray `[4, -1, 2, 1]`)

**Code Outline**:
```python
def kadane(arr):
    if not arr: return 0
    current_max = global_max = arr[0]
    for num in arr[1:]:
        current_max = max(num, current_max + num)
        global_max = max(global_max, current_max)
    return global_max
```

## Important Insights and Definitions
- **Negative Numbers**: Works with negative numbers because we reset `current_max` if adding the current element worsens the sum.
- **All Negative Arrays**: Returns the largest (least negative) element if all are negative.
- **Variants**:
  - **Maximum Product Subarray**: Similar idea but track both max and min.
  - **Circular Array**: Consider wrapping around by using total sum minus minimum subarray sum.
  - **With Constraints**: For example, length of subarray must be at least L.
- **Empty Subarray**: Standard Kadane's doesn't consider empty subarrays; modify to allow zero if needed.

## Differences from Other Patterns
- **vs. Prefix Sum**: Prefix sum precomputes sums for O(1) queries, while Kadane's finds a single best contiguous sum in O(1) space.
- **vs. Sliding Window**: Sliding window maintains a specific condition (e.g., size K), while Kadane's allows any contiguous subarray.
- **vs. Two Pointers**: Two pointers often look for pairs or specific boundaries, while Kadane's is designed for sum optimization.

## Template Questions to Remember When to Use
- Does the problem ask for the "maximum sum of a contiguous subarray"?
- Are there negative numbers and you need to efficiently find the best sum?
- Is the problem asking for something like: "Find the subarray with the maximum sum" or "Find maximum profit given a sequence of daily changes"?

## Type of Questions Solved
- **Maximum Subarray**: Typical variant as described.
- **Maximum Sum of Subarray with constraints** (e.g., at least length L).
- **Circular array** maximum sum (House Robber II style).
- **Dynamic programming** foundation for more complex sum/product problems.
- **Financial applications**: Maximum profit from price changes (stock prices daily differences).

## Complexity
- **Time**: O(n) — single pass.
- **Space**: O(1) — constant extra variables.

---
*Key Insight: Kadane's algorithm works because it localizes the optimal solution: any subarray ending at position i either starts at i or extends the optimal subarray ending at i-1. That's the dynamic programming substructure.*