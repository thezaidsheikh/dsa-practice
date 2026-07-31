# RisingBrain Problem Note Template

Standard structure for problem notes in this folder. RisingBrain-track notes group problems by topic (e.g., `Array/Two-Pointers/`) and use a compact, solution-first format.

## How to Use This Template

1. First line: `Prob:` followed by the LeetCode problem URL.
2. Always write three solution tiers: `Sol 1: Brute Force`, `Sol 2: Better`, `Sol 3: Optimal`.
3. Keep the numbered steps short and action-based.
4. Use Java. Name solution classes descriptively when multiple approaches share a file (e.g., `SolutionBruteForce`, `SolutionOptimized`); use `class Solution` when only one block exists.
5. Close each solution with complexity lines in `Time Complexity:` / `Space Complexity:` style.
6. Place the note in the topic folder that matches the problem's pattern (e.g., `Two-Pointers/`).

## Template

````md
Prob: <problem-url>

Sol 1: Brute Force - <approach-name>

1. <step>
2. <step>

```java
class Solution {
    public <return-type> <method>(<args>) {
        // code
    }
}
```

Time Complexity: O(...),
Space Complexity: O(...)

Sol 2: Better - <approach-name>

1. <step>
2. <step>

```java
class Solution {
    // code
}
```

Time Complexity: O(...),
Space Complexity: O(...)

Sol 3: Optimal - <approach-name>

# Intuition

1. <step>
2. <step>

```java
class Solution {
    // code
}
```

Time Complexity: O(...),
Space Complexity: O(...)

```

```
````

## Filled Example (3Sum)

````md
Prob: https://leetcode.com/problems/3sum/description/

Sol 1: Brute Force - Using 3 nested loops

1. For every i, j, k in the array, check if nums[i] + nums[j] + nums[k] == 0.
2. Sort each valid triplet and add it to a set to avoid duplicates.
3. Return all unique triplets.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        Set<List<Integer>> set = new HashSet<>();
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> triplet = Arrays.asList(nums[i], nums[j], nums[k]);
                        Collections.sort(triplet);
                        set.add(triplet);
                    }
                }
            }
        }
        return new ArrayList<>(set);
    }
}
```
````

Time Complexity: O(n^3),
Space Complexity: O(k) // k = number of triplets

Sol 2: Better - Using a hash set per fixed element

1. Fix the first element (i).
2. Use a set to find pairs (j, k) that sum to -nums[i].
3. Add unique triplets to the result.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> res = new HashSet<>();
        for (int i = 0; i < nums.length; i++) {
            Set<Integer> seen = new HashSet<>();
            for (int j = i + 1; j < nums.length; j++) {
                int complement = -nums[i] - nums[j];
                if (seen.contains(complement)) {
                    List<Integer> t = Arrays.asList(nums[i], nums[j], complement);
                    Collections.sort(t);
                    res.add(t);
                }
                seen.add(nums[j]);
            }
        }
        return new ArrayList<>(res);
    }
}
```

Time Complexity: O(n^2),
Space Complexity: O(n)

Sol 3: Optimal - Sort + two pointers

1. Sort the array so two pointers work on the remaining part.
2. Fix one element and use left/right pointers to find pairs summing to -nums[i].
3. Skip duplicates while moving the pointers.
4. Move left/right based on whether the sum is less than or greater than the target.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            int left = i + 1, right = n - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == 0) {
                    res.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    while (left < right && nums[left] == nums[left + 1]) left++;
                    while (left < right && nums[right] == nums[right - 1]) right--;
                    left++;
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    right--;
                }
            }
        }
        return res;
    }
}
```

Time Complexity: O(n^2),
Space Complexity: O(k) // k = number of triplets

```

```
