# Prefix Sum Technique

## Definition
Prefix Sum (also called Cumulative Sum) is a technique where you precompute an array such that each element at index `i` contains the sum of all elements from the start of the array up to (and including) index `i`. This allows O(1) range sum queries and is extremely powerful for solving subarray sum problems efficiently.

## When and Why to Use
- **When**: You need to answer multiple queries about subarray sums, or solve problems where subarray sums appear repeatedly (e.g., "find subarrays that sum to K", "range sum queries").
- **Why**: Converts O(n) per query into O(1) after O(n) preprocessing, dramatically reducing time complexity for multiple queries.

## How It Works
### Step-by-Step:
1. **Precompute** a prefix sum array `prefix` of length `n+1` where:
   - `prefix[0] = 0`
   - `prefix[i] = prefix[i-1] + arr[i-1]` for `i` from 1 to `n`
2. **Range Sum Query**: Sum of subarray from index `l` to `r` (inclusive) is:
   ```
   sum(l, r) = prefix[r+1] - prefix[l]
   ```

### Simple Example: Range Sum Query
**Problem**: Given an array, answer many queries: "What is the sum of elements from index L to R?"
**Solution**:
- Precompute prefix sums once.
- Each query answered in O(1) time using `prefix[R+1] - prefix[L]`.

**Example Code**:
```python
arr = [3, 6, 2, 8, 7, 5, 4]
prefix = [0] * (len(arr) + 1)
for i in range(1, len(prefix)):
    prefix[i] = prefix[i-1] + arr[i-1]

# Query sum from index 2 to 5
l, r = 2, 5
result = prefix[r+1] - prefix[l]   # O(1)
```

## Important Insights and Definitions
- **Padding**: Extra zero at `prefix[0]` simplifies formulas.
- **Subarray Sum = K**: If you want to count subarrays summing to K, use `prefix[j] - prefix[i] = K` → `prefix[i] = prefix[j] - K`. Use a hashmap to store frequencies of prefix sums encountered.
- **Handles Negative Numbers**: Works with negative numbers; useful for counting subarrays with given sum.
- **2D Prefix Sum**: Extends to 2D matrices for O(1) submatrix sum queries (integral image concept).

## Differences from Other Patterns
- **vs. sliding window**: Sliding window adjusts window size to maintain a condition (e.g., at most K distinct), while prefix sum precomputes for O(1) range queries. Prefix sum doesn't inherently enforce conditions on subarrays.
- **vs. Kadane's algorithm**: Kadane's finds the maximum sum of any contiguous subarray in O(1) space. Prefix sum can find maximum sum subarray (by scanning for `max(prefix[j] - min(prefix[i]))`), but needs O(n) space. However, prefix sum is more versatile for counting problems (e.g., number of subarrays summing to K).
- **vs. two pointers**: Two pointers technique avoids nested loops without extra space; prefix sum adds O(n) space for capabilities like counting subarrays matching a condition.

## Template Questions to Remember When to Use
- Do I need to quickly compute the sum of any subarray (range sum queries)?
- Is the problem about counting the number of subarrays with a specific sum (e.g., equals K, divisible by K)?
- Does the problem involve "sum of elements between L and R in many queries"?
- Can I transform the problem into checking `prefix[j] - prefix[i] = target`?

## Type of Questions Solved
- **Range Sum Queries**: Multiple sum queries on static array (Leetcode 303: Range Sum Query - Immutable).
- **Count subarrays with sum K**: Count the number of subarrays whose sum equals a given integer.
- **Subarray sum divisible by K**: Count subarrays where sum % K == 0 (Leetcode 974).
- **Maximum subarray sum**: Can be solved with prefix sum by maintaining min prefix seen so far.
- **Equilibrium index**: Index where sum of left elements equals sum of right elements (use prefix sums for O(n)).
- **2D matrix range sum** (using 2D prefix sum): Compute sum of any submatrix quickly.

## Complexity
- **Preprocessing**: O(n) time and O(n) additional space.
- **Query**: O(1) time per range sum query.
- **Counting subarrays with sum K**: O(n) time, O(n) space (hashmap).

---
*Key Insight: Prefix sum transforms a range sum problem into a difference of two numbers. The magic happens when you realize that `sum(L..R) = prefix[R] - prefix[L-1]`. This enables powerful counting via hashmap when combined with the equation `prefix[j] - prefix[i] = target`.*