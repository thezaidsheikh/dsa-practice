# Two Sum

## Problem Statement
Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`. You may assume that each input would have exactly one solution, and you may not use the same element twice.

### Example
```
Input: nums = [2, 7, 11, 15], target = 9
Output: [0, 1]
Explanation: Because nums[0] + nums[1] = 2 + 7 = 9.
```

## Brute Force Approach
A straightforward solution checks every possible pair:
```python
def two_sum_brute(nums, target):
    n = len(nums)
    for i in range(n):
        for j in range(i + 1, n):
            if nums[i] + nums[j] == target:
                return [i, j]
    return []
```
- **Time Complexity:** O(n²)
- **Space Complexity:** O(1)

## Optimal Approach Using Hash Map
We can reduce the time complexity to O(n) by storing each number's index in a hash map while iterating through the array. For each element, we check if the complement (`target - nums[i]`) already exists in the map. If it does, we have found the solution.

```python
def two_sum(nums, target):
    num_map = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in num_map:
            return [num_map[complement], i]
        num_map[num] = i
    return []
```
- **Time Complexity:** O(n)
- **Space Complexity:** O(n) for the hash map

## Step-by-Step Walkthrough
Let's walk through the example `nums = [2, 7, 11, 15]`, `target = 9`:
1. Initialize an empty hash map: `{}`
2. Iteration 0: `num = 2`
   - Complement = `9 - 2 = 7`
   - `7` is not in the map, so add `2: 0` to the map.
   - Map becomes `{2: 0}`
3. Iteration 1: `num = 7`
   - Complement = `9 - 7 = 2`
   - `2` **is** in the map (index 0)
   - Return `[0, 1]` as the result

## Variations and Follow‑Up Questions
- **Multiple Solutions?** The problem guarantees exactly one solution, but if multiple pairs were possible, you could return any one of them.
- **Modulo Variant:** If the problem asked for indices modulo some value, you would adjust the return statement accordingly.
- **Hash Map with Default Values:** In languages like C++ you can use `unordered_map<int, int>` and check existence with `find()`.
- **Extended Input Types:** For floating‑point targets, consider using a tolerance when checking complements.

## Common Pitfalls
- **Using the Same Element Twice:** Ensure you check the complement **before** inserting the current number into the map.
- **Order of Return Values:** The indices must be returned in the order of the complement's index first, then the current index.
- **Handling Large Inputs:** The O(n) solution scales linearly, making it suitable for inputs up to 10⁵ elements.

## Summary
The Two Sum problem introduces hash maps as a powerful tool for reducing nested loop complexities. By storing each element's index and checking for its complement in constant time, we achieve an optimal O(n) solution.

---
*Prepared for the Algorithms‑Patterns repository.*