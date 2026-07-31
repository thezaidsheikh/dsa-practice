# Two Pointers Technique

## Definition
The two pointers technique is a pattern where two pointers (indices) are used to traverse an array or list, typically moving towards each other or in the same direction, to solve problems efficiently in linear time.

## When and Why to Use
- **When**: Problems involving arrays or linked lists where you need to find pairs, triplets, or subarrays that satisfy certain conditions (e.g., sum equals a target, palindrome check, removing duplicates).
- **Why**: It reduces time complexity from O(n²) or O(n³) to O(n) by avoiding nested loops, and uses O(1) extra space.

## How It Works
Two indices scan the data in a single pass. At every step a deterministic rule tells us which pointer to move, and that move permanently eliminates one element from future consideration. Because no element is examined more than a constant number of times, the total work is linear.

### Common Variations
1. **Pointers moving towards each other** (start and end):
   - Used for sorted arrays to find pairs with a given sum.
   - Example: Given a sorted array, find two numbers that add up to a target.
2. **Pointers moving in the same direction** (slow and fast):
   - Used for removing duplicates, detecting cycles, or finding midpoints.
   - Example: Remove duplicates from a sorted array.
3. **Three pointers (left, mid, right)**:
   - Used for partitioning problems like Dutch National Flag (Sort Colors).
   - Example: Sort an array of 0s, 1s, and 2s in linear time without extra space.

### Simple Example: Two Sum II (Sorted Array)
**Problem**: Given a sorted array of integers, find two numbers such that they add up to a specific target.
**Approach**:
- Initialize left pointer at start (0) and right pointer at end (n-1).
- While left < right:
  - Calculate current sum = arr[left] + arr[right].
  - If sum == target, return the pair.
  - If sum < target, increment left (to increase sum).
  - If sum > target, decrement right (to decrease sum).

**Code Outline (Python)**:
```python
def two_sum_sorted(arr, target):
    left, right = 0, len(arr) - 1
    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return [left, right]
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    return [-1, -1]  # Not found
```

### Template / Code Implementation (Java)
```java
public int[] twoSum(int[] numbers, int target) {
    int left = 0, right = numbers.length - 1;
    while (left < right) {
        int sum = numbers[left] + numbers[right];
        if (sum == target) {
            return new int[]{left, right};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return new int[]{-1, -1}; // Not found
}
```

## Important Insights and Definitions
- **Sorted Requirement**: For the "towards each other" variation, the array must be sorted. If not, sort first (O(n log n)) or use a hash map (O(n) time, O(n) space).
- **Duplicate Handling**: When finding all unique pairs, skip duplicates after finding a valid pair to avoid duplicate results.
- **Linked List Adaptation**: In linked lists, use two pointers where one moves faster (e.g., fast and slow pointers for cycle detection).
- **Positive numbers constraint (subarray sum only)**: For contiguous subarray problems using a two-pointer sliding window (e.g., subarray sum equals k), all numbers must be non-negative. With negatives, expanding the window can decrease the sum, breaking the monotonicity needed for deterministic pointer movement. Pair-based two-pointer (e.g., Two Sum) works fine with negatives as long as the array is sorted — this distinction is a common interview trap.
- **Deterministic movement required**: At each step there must be exactly one correct pointer to move. If both advancing left and retreating right could lead to a valid solution, the technique cannot be applied deterministically.
- **One elimination per step (why O(n))**: Each iteration eliminates one element from future consideration — the element the moved pointer passes over can never be part of a valid solution. This is the core reason the algorithm runs in O(n).
- **k-sum generalization**: For finding k numbers that satisfy a condition (e.g., 3Sum, 4Sum), fix k-2 elements with nested loops and use two pointers on the remaining segment. This yields O(n^(k-1)) instead of O(n^k).
- **Two separate arrays**: Two pointers can also traverse two different sorted arrays simultaneously (e.g., merging sorted arrays, finding intersection of two arrays).

## Complexity Analysis
- **Time**: O(n) for a single pass, O(n log n) if sorting is required first.
- **Space**: O(1) extra space (not counting the space for sorting if done in-place).

## Edge Cases to Watch For
- **Unsorted array**: Sort first (O(n log n)) or fall back to a hash map.
- **Duplicates**: Skip repeated values after a valid pair so the result set stays unique.
- **No valid pair**: Return a sentinel such as [-1, -1].
- **Negatives in pair problems**: Fine — sorted order keeps the sum monotonic.
- **Negatives in subarray-sum problems**: The two-pointer window breaks; use prefix sum + hash map instead.

## Differences from Other Patterns
- **vs. Sliding Window**: Two pointers often look for a specific condition (like a sum) and may not maintain a fixed window size. Sliding window maintains a window that satisfies a condition (e.g., contains at most K distinct characters) and adjusts the window size.
- **vs. Prefix Sum**: Prefix sum is used for subarray sum queries and requires preprocessing. Two pointers works on the original array without extra space (if sorted) and is more flexible for pairwise conditions.
- **vs. Kadane's Algorithm**: Kadane's is specifically for maximum subarray sum. Two pointers can be adapted for similar problems but is more general.

## Recognition Cues / Template Questions
- Is the array sorted? (If yes, two pointers towards each other is a strong candidate)
- Do I need to find a pair or triplet that meets a condition? (Two pointers can reduce O(n²) to O(n))
- Am I trying to remove duplicates or find a midpoint in a linked list? (Slow-fast pointers)
- Can I define a condition that tells me whether to move the left or right pointer?
- Are all numbers non-negative and you need a subarray matching a target sum? (Sliding window two-pointer works here)
- Do I need to partition an array with a few distinct values? (Three-pointer / Dutch National Flag applies)

## Type of Questions Solved
- **Pair Problems**: Two Sum, Three Sum, Four Sum (with sorting and two pointers)
- **Array Manipulation**: Remove Duplicates, Sort Colors (Dutch National Flag)
- **Linked List**: Cycle Detection, Middle of Linked List, Palindrome Linked List
- **Others**: Container With Most Water, Trapping Rain Water (variations)

## Limitations / When Not to Use
- **Unsorted and cannot be modified**: If the array is unsorted and sorting in-place is not allowed, the opposite-direction variation cannot be used directly. A hash map is usually the alternative.
- **Negative numbers in subarray sum**: Two-pointer sliding window for finding a target subarray sum only works with non-negative numbers. For arrays with negatives, use prefix sum with a hash map instead.
- **No deterministic movement rule**: If both advancing and retreating a pointer could potentially satisfy the condition, two pointers cannot guarantee finding a solution.
- **Random access required for opposite-direction**: The start-and-end pattern relies on random access to elements (done by index). Singly linked lists cannot use this variation directly without first converting to an array (O(n) extra space).
- **All pairs needed**: If the problem requires enumerating every possible pair (not just finding one solution or skipping invalid ones), nested loops or a hash map are better suited.

## Key Takeaways
- Two pointers is about leveraging order (sorted or sequential) to eliminate unnecessary checks.
- At every step exactly one pointer move must be the correct choice — that determinism is what guarantees linear time.
