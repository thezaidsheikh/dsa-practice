# Two Pointers Technique

## Definition
The two pointers technique is a pattern where two pointers (indices) are used to traverse an array or list, typically moving towards each other or in the same direction, to solve problems efficiently in linear time.

## When and Why to Use
- **When**: Problems involving arrays or linked lists where you need to find pairs, triplets, or subarrays that satisfy certain conditions (e.g., sum equals a target, palindrome check, removing duplicates).
- **Why**: It reduces time complexity from O(n²) or O(n³) to O(n) by avoiding nested loops, and uses O(1) extra space.

## How It Works
### Common Variations:
1. **Pointers moving towards each other** (start and end):
   - Used for sorted arrays to find pairs with a given sum.
   - Example: Given a sorted array, find two numbers that add up to a target.
2. **Pointers moving in the same direction** (slow and fast):
   - Used for removing duplicates, detecting cycles, or finding midpoints.
   - Example: Remove duplicates from a sorted array.

### Simple Example: Two Sum II (Sorted Array)
**Problem**: Given a sorted array of integers, find two numbers such that they add up to a specific target.
**Approach**:
- Initialize left pointer at start (0) and right pointer at end (n-1).
- While left < right:
  - Calculate current sum = arr[left] + arr[right].
  - If sum == target, return the pair.
  - If sum < target, increment left (to increase sum).
  - If sum > target, decrement right (to decrease sum).

**Code Outline**:
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

## Important Insights and Definitions
- **Sorted Requirement**: For the "towards each other" variation, the array must be sorted. If not, sort first (O(n log n)) or use a hash map (O(n) time, O(n) space).
- **Duplicate Handling**: When finding all unique pairs, skip duplicates after finding a valid pair to avoid duplicate results.
- **Linked List Adaptation**: In linked lists, use two pointers where one moves faster (e.g., fast and slow pointers for cycle detection).

## Differences from Other Patterns
- **vs. Sliding Window**: Two pointers often look for a specific condition (like a sum) and may not maintain a fixed window size. Sliding window maintains a window that satisfies a condition (e.g., contains at most K distinct characters) and adjusts the window size.
- **vs. Prefix Sum**: Prefix sum is used for subarray sum queries and requires preprocessing. Two pointers works on the original array without extra space (if sorted) and is more flexible for pairwise conditions.
- **vs. Kadane's Algorithm**: Kadane's is specifically for maximum subarray sum. Two pointers can be adapted for similar problems but is more general.

## Template Questions to Remember When to Use
- Is the array sorted? (If yes, two pointers towards each other is a strong candidate)
- Do I need to find a pair or triplet that meets a condition? (Two pointers can reduce O(n²) to O(n))
- Am I trying to remove duplicates or find a midpoint in a linked list? (Slow-fast pointers)
- Can I define a condition that tells me whether to move the left or right pointer?

## Type of Questions Solved
- **Pair Problems**: Two Sum, Three Sum, Four Sum (with sorting and two pointers)
- **Array Manipulation**: Remove Duplicates, Sort Colors (Dutch National Flag)
- **Linked List**: Cycle Detection, Middle of Linked List, Palindrome Linked List
- **Others**: Container With Most Water, Trapping Rain Water (variations)

## Complexity
- **Time**: O(n) for single pass, O(n log n) if sorting is required first.
- **Space**: O(1) extra space (not counting the space for sorting if done in-place).

---
*Remember: Two pointers is about leveraging order (sorted or sequential) to eliminate unnecessary checks.*