# Algorithm Study Template

A standard structure for learning and documenting any algorithm or topic in depth.

## How to Use This Template
1. Copy this file (or just its headings) when studying a new algorithm.
2. Fill every section that applies. Skip a section only if it genuinely does not apply to the topic.
3. Keep the code in Java by default, unless the topic or local folder convention suggests otherwise.
4. Verify every complexity claim before writing it down.
5. End with a "Key Takeaways" note so the topic is easy to revise later.

---

## [Algorithm Name]

### Definition
- What does this algorithm do? (1-3 sentences)
- What problem family does it belong to? (searching, sorting, optimization, etc.)

### When and Why to Use
- **When**: What input/output conditions make this the right choice?
- **Why**: What does it give us over the obvious alternative? (e.g., O(n²) -> O(n))

### How It Works
- Explain the core mechanism step by step in plain English.
- What data structure(s) does it rely on and why?
- What is the invariant that the algorithm maintains at every step?

#### Common Variations
- List the main variants of this algorithm and when each is used.
- Example: Two pointers -> opposite direction, same direction, three pointers.

#### Simple Example
- Use a small, concrete example and trace through it by hand.
- Show the state of the data at each step.

#### Template / Code Outline
```java
// Pseudo/actual code outline — fill for the specific algorithm.
```

### Important Insights and Definitions
- Key observations that make the algorithm work.
- Critical definitions, assumptions, or preconditions.
- Any subtle trap or common misconception to avoid.

### Complexity Analysis
- **Time**: Best / Average / Worst case, with reasoning.
- **Space**: Auxiliary space, with reasoning.
- Note whether output storage or recursion stack space is being counted.

### Edge Cases to Watch For
- Empty input.
- Single element.
- All elements equal / all distinct.
- Duplicates.
- Extremely large or small values (overflow risk).
- Sorted vs. unsorted input.

### Differences from Other Patterns
- How is this different from similar or nearby techniques?
- When should you pick one over the other?

### Recognition Cues / Template Questions
- Question 1 to ask yourself to recognize this pattern.
- Question 2 to ask yourself...
- The fastest signals in a problem statement that point to this algorithm.

### Type of Questions Solved
- List concrete problem examples (LeetCode-style names) that this algorithm solves.
- Group them by sub-category.

### Limitations / When Not to Use
- When does this algorithm fail or become worse than an alternative?
- What input constraints break its assumptions?

### Key Takeaways
- One-line summary of the algorithm.
- The single most important thing to remember in an interview.
- Anything you kept confusing it with before.

---

## Filled Example (Two Pointers)

Below is what this template looks like when fully filled in, using the two pointers technique as the reference example. New algorithm notes should follow this level of depth.

### Definition
The two pointers technique is a pattern where two pointers (indices) traverse an array or list, typically moving towards each other or in the same direction, to solve problems in linear time.

### When and Why to Use
- **When**: Problems involving arrays or linked lists where you need pairs, triplets, or subarrays that satisfy a condition (sum equals target, palindrome check, removing duplicates).
- **Why**: It reduces O(n²) or O(n³) solutions to O(n) by avoiding nested loops, using O(1) extra space.

### How It Works
Maintain two indices and use a deterministic rule (based on the current state) to move one pointer per step. The key invariant: each moved pointer permanently eliminates one element from consideration.

#### Common Variations
1. Opposite direction (left/right): sorted arrays, pair-sum problems.
2. Same direction (slow/fast): remove duplicates, cycle detection, find midpoint.
3. Three pointers (left/mid/right): Dutch National Flag partitioning.

#### Template / Code Outline
```java
public int[] twoSumSorted(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target) return new int[]{left, right};
        if (sum < target) left++;
        else right--;
    }
    return new int[]{-1, -1};
}
```

### Important Insights and Definitions
- **Sorted requirement**: Opposite-direction pointers need a sorted array (sort first if not).
- **Positive numbers (subarray sum only)**: Sliding-window sum problems need non-negative numbers; pair-sum problems work fine with negatives.
- **Deterministic movement**: At every step exactly one pointer move must be correct, or the technique fails.

### Complexity Analysis
- **Time**: O(n) for a single pass; O(n log n) if sorting is required first.
- **Space**: O(1) auxiliary (excluding in-place sorting).

### Edge Cases
- No valid pair exists -> return sentinel.
- Duplicates -> skip to avoid duplicate results.
- Unsorted input -> sort first or use a hash map.
- Negatives in subarray-sum problems -> switch to prefix sum + hash map.

### Recognition Cues
- "Is the array sorted?" -> opposite-direction pointers are a strong candidate.
- "Find a pair/triplet meeting a condition?" -> two pointers reduce nested loops.
- "Non-negative numbers + subarray with target sum?" -> sliding window works.
- "Cycle detection / midpoint in a linked list?" -> slow-fast pointers.

### Type of Questions Solved
- **Pair problems**: Two Sum, Three Sum, Four Sum.
- **Array manipulation**: Remove Duplicates, Sort Colors.
- **Linked list**: Cycle Detection, Middle of Linked List, Palindrome Linked List.
- **Others**: Container With Most Water, Trapping Rain Water.

### Limitations / When Not to Use
- Unsorted array that cannot be modified.
- Negative numbers in subarray-sum problems.
- No deterministic pointer-movement rule.
- Enumerating all pairs rather than finding valid ones.

### Key Takeaways
Two pointers is about leveraging order (sorted or sequential) to eliminate unnecessary checks, which is why it achieves linear time with constant extra space.
