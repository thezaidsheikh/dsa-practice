# Sliding Window Technique

## Definition
A technique that uses two pointers to create a "window" over an array or string. The window slides (both pointers move forward) to efficiently find subarrays/substrings satisfying certain conditions, avoiding redundant recomputation.

## When to Use
- Problems asking for: maximum/minimum/longest/shortest subarray or substring
- Contiguous elements with a specific property
- Reducing O(n²) brute-force to O(n)

## Why It Works
Instead of recalculating from scratch for each position, we maintain state from the previous window and adjust incrementally.

---

## Fixed-Size Sliding Window

Window size is constant (k). Move the window one step at a time, updating the result in O(1).

### Pattern
```java
int windowSum = 0;
for (int i = 0; i < k; i++) {
    windowSum += arr[i];
}

for (int i = k; i < n; i++) {
    windowSum += arr[i] - arr[i - k];
    // evaluate window
}
```

### Example: Maximum Sum Subarray of Size K
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

---

## Variable-Size Sliding Window

Window size expands/shrinks based on a condition. Common for "at most", "at least", or "longest/shortest" problems.

### Pattern
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

### Example: Longest Substring with At Most K Distinct Characters
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

### Example: Minimum Size Subarray Sum (At Least Target)
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

---

## Common Question Types

| Type | Condition | Example |
|------|-----------|---------|
| Maximum sum (fixed k) | Sum of k elements | Max sum subarray of size k |
| Longest substring | At most K distinct | Longest with ≤ 2 distinct chars |
| Shortest subarray | At least sum S | Min length ≥ target sum |
| Longest repeating | All unique | Longest without repeating chars |
| Window contains | Target chars | Minimum window substring |

---

## Differences From Related Patterns

| Pattern | Key Difference |
|---------|----------------|
| Two Pointers | Sliding window has a bounded window; two pointers find pairs without fixed size |
| Prefix Sum | Prefix sum answers queries; sliding window maintains a dynamic constraint |
| Kadane's | Kadane's handles negative numbers optimally; sliding window requires a constraint |

---

## Complexity
- **Time**: O(n) - each element enters and exits the window at most once
- **Space**: O(1) for fixed-size; O(min(n, charset)) for variable-size with hashmap

## Recognition Cues
- "Find the longest/shortest subarray/substring with..."
- "Maximum sum of k consecutive elements"
- "Contains at most/at least K..."
- "Subarray sum equals/at least..."

---

## Key Takeaway
Sliding window turns nested loops into a single pass by maintaining state. Use **fixed** when the size is given; use **variable** when you need to find the best window that satisfies a condition.
