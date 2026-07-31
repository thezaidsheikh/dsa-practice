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

**Example Code (Python)**:
```python
arr = [3, 6, 2, 8, 7, 5, 4]
prefix = [0] * (len(arr) + 1)
for i in range(1, len(prefix)):
    prefix[i] = prefix[i-1] + arr[i-1]

# Query sum from index 2 to 5
l, r = 2, 5
result = prefix[r+1] - prefix[l]   # O(1)
```

### Code Implementation (Java)
```java
class NumArray {
    private int[] prefix;

    public NumArray(int[] nums) {
        prefix = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }
    }

    public int sumRange(int left, int right) {
        return prefix[right + 1] - prefix[left];
    }
}
```

## Important Insights and Definitions
- **Padding**: Extra zero at `prefix[0]` simplifies formulas.
- **Subarray Sum = K**: If you want to count subarrays summing to K, use `prefix[j] - prefix[i] = K` → `prefix[i] = prefix[j] - K`. Use a hashmap to store frequencies of prefix sums encountered.
- **Handles Negative Numbers**: Works with negative numbers; useful for counting subarrays with given sum.
- **2D Prefix Sum**: Extends to 2D matrices for O(1) submatrix sum queries (integral image concept).

### Counting Subarrays with Sum K (Java)
```java
public int subarraySum(int[] nums, int k) {
    HashMap<Integer, Integer> map = new HashMap<>();
    map.put(0, 1); // empty prefix sum
    int sum = 0, count = 0;
    for (int num : nums) {
        sum += num;
        count += map.getOrDefault(sum - k, 0);
        map.put(sum, map.getOrDefault(sum, 0) + 1);
    }
    return count;
}
```

## Complexity Analysis
- **Preprocessing**: O(n) time and O(n) additional space.
- **Query**: O(1) time per range sum query.
- **Counting subarrays with sum K**: O(n) time, O(n) space (hashmap).

## Edge Cases to Watch For
- **Empty array**: prefix array is just `[0]`; queries are invalid.
- **l = 0**: formula `prefix[r+1] - prefix[0]` still works thanks to the padding zero.
- **Integer overflow**: with large arrays, sums can overflow int — consider `long` where needed.
- **Negative numbers**: handled naturally; for "sum % K == 0" counting, adjust the modulo so negative remainders are handled correctly.
- **Frequently updated arrays**: prefix sums go stale after every update — use a Fenwick tree (BIT) instead.

## Differences from Other Patterns
- **vs. sliding window**: Sliding window adjusts window size to maintain a condition (e.g., at most K distinct), while prefix sum precomputes for O(1) range queries. Prefix sum doesn't inherently enforce conditions on subarrays.
- **vs. Kadane's algorithm**: Kadane's finds the maximum sum of any contiguous subarray in O(1) space. Prefix sum can find maximum sum subarray (by scanning for `max(prefix[j] - min(prefix[i]))`), but needs O(n) space. However, prefix sum is more versatile for counting problems (e.g., number of subarrays summing to K).
- **vs. two pointers**: Two pointers technique avoids nested loops without extra space; prefix sum adds O(n) space for capabilities like counting subarrays matching a condition.

## Recognition Cues / Template Questions
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

## Limitations / When Not to Use
- **Frequently updated arrays**: Prefix sums go stale after every update; use a Fenwick tree (BIT) for point-update range-sum queries.
- **O(n) space cost**: If O(1) auxiliary space is required, two pointers or Kadane's may be better — prefix sum trades space for query speed.
- **Enforcing complex conditions**: Prefix sum computes sums but doesn't inherently enforce constraints like "at most K distinct characters"; that's sliding window territory.
- **Large sums**: Watch for overflow; switch to `long` when values are big.

## Key Takeaways
- Prefix sum transforms a range sum problem into a difference of two numbers: `sum(L..R) = prefix[R+1] - prefix[L]`.
- Combined with a hash map, it powers counting problems via `prefix[j] - prefix[i] = target`, and it handles negative numbers naturally.
