# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview
This is a Data Structures and Algorithms (DSA) practice repository containing solutions organized by problem types and difficulty levels. The repository focuses on algorithmic problem-solving with implementations in various categories.

## Folder Structure and Content
### Core Problem Categories
- **Binary-Search/**: Solutions for rotated sorted array problems (Find Minimum, Search in Rotated Sorted Array)
- **Striver/**: Striver's curated array problems (Rotate Array, Spiral Matrix, Leaders in Array)
- **Leetcode-150/**: LeetCode 150 problems (Merge Sorted Array, Remove Element, Majority Element)
- **Heap/**: Heap-related problems (Kth Largest Element, Top K Frequent Elements, Task Scheduler)
- **Tree/**: Tree traversal and manipulation (Inorder/Preorder/Postorder, Level Order, LCA, BST operations)
- **Recursion/**: Recursive problem solutions (Fibonacci, Generate Parentheses, Combination Sum)
- **Prefix-Sum/**: Subarray sum problems (Subarray Sum Equals K, Maximum Subarray Sum)

### Additional Patterns
- **Two Pointers**: Solutions for problems like Two Sum, Container With Most Water
- **Sliding Window**: Solutions for problems like Longest Substring Without Repeating Characters
- **Kadane's Algorithm**: Solutions for maximum subarray sum problems

## Development Commands
Since this is a documentation and practice repository without executable code, no specific build or test commands are applicable. The repository primarily contains:
1. **Markdown files** documenting problem solutions
2. **Algorithm explanations** with step-by-step reasoning
3. **Code patterns** for common DSA problems

## Key Patterns and Approaches
### Two Pointers Technique
- Used for problems involving arrays or linked lists where you need to find pairs, triplets, or subarrays that satisfy certain conditions (e.g., sum equals a target, palindrome check, removing duplicates).
- Reduces time complexity from O(n²) or O(n³) to O(n) by avoiding nested loops, and uses O(1) extra space.

### Sliding Window Technique
- Maintains a window of elements in an array or string that satisfies a specific condition, and dynamically adjusts the window size by sliding it across the data structure.
- Reduces O(n²) brute-force solutions to O(n) by reusing computations from previous windows.

### Kadane's Algorithm
- Finds the maximum sum of a contiguous subarray in O(n) time and O(1) space.
- Works with negative numbers and is the foundation for many other variations.

### Prefix Sum Technique
- Precomputes an array such that each element at index `i` contains the sum of all elements from the start of the array up to (and including) index `i`.
- Allows O(1) range sum queries after O(n) preprocessing.

## Important Notes
- This repository is for learning and documentation purposes
- Solutions are primarily documented in markdown format
- No actual Python/JavaScript/Java code files are present
- Focus is on algorithmic thinking and problem-solving approaches
- Each solution includes detailed explanations and complexity analysis

---
*Key Insight: The patterns in this repository provide a systematic approach to solving DSA problems efficiently. Each pattern has its own strengths and is best suited for specific types of