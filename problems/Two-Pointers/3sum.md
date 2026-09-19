Prob: https://leetcode.com/problems/3sum/

Sol 1: Brute Force - Check all triplets
1. Run three nested loops and pick every possible triplet `(i, j, k)` where `i < j < k`.
2. If `nums[i] + nums[j] + nums[k] == 0`, sort the triplet and store it only if it is not already present.
3. Return all unique triplets collected this way.

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
Time complexity - O(n^3),
Space complexity - O(m), where m is the number of unique triplets stored

Sol 2: Better - Fix one index + HashSet two-sum
1. Fix `i` and reduce the problem to finding two numbers in the suffix that sum to `-nums[i]`.
2. Traverse `j` from `i + 1` onward while maintaining a set of seen numbers.
3. For each `nums[j]`, check whether `target - nums[j]` exists in the set.
4. If yes, build the triplet, sort it, and add it to a global set to avoid duplicates.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        Set<List<Integer>> ans = new HashSet<>();

        for (int i = 0; i < n; i++) {
            HashSet<Integer> seen = new HashSet<>();
            int target = -nums[i];

            for (int j = i + 1; j < n; j++) {
                int third = target - nums[j];
                if (seen.contains(third)) {
                    List<Integer> triplet = Arrays.asList(nums[i], nums[j], third);
                    Collections.sort(triplet);
                    ans.add(triplet);
                }
                seen.add(nums[j]);
            }
        }

        return new ArrayList<>(ans);
    }
}
```
Time complexity - O(n^2 * log 3) ~ O(n^2),
Space complexity - O(n) for the seen set (plus result storage)

Sol 3: Optimal - Sorting + Two Pointers
# Intuition
After sorting, fixing one number lets us search for the other two with opposite movement pointers. If the current sum is small, moving the left pointer increases it; if large, moving the right pointer decreases it. Sorted order also makes duplicate skipping straightforward, which ensures unique triplets.

1. Sort the array.
2. Iterate `i` from `0` to `n - 3`, and skip duplicate fixed values.
3. Set `j = i + 1` and `k = n - 1`.
4. Compare `nums[j] + nums[k]` with `-nums[i]` and move pointers accordingly.
5. On a match, store triplet, move both pointers, and skip duplicates on both sides.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();

        int n = nums.length;

        for (int i = 0; i < n - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;

            int sum = -1 * nums[i];
            int j = i + 1;
            int k = n - 1;

            while (j < n && j < k) {
                int sum2 = nums[j] + nums[k];

                if (sum2 == sum) {
                    List<Integer> ls = new ArrayList<>();
                    ls.add(nums[i]);
                    ls.add(nums[j]);
                    ls.add(nums[k]);
                    res.add(ls);

                    j++;
                    k--;

                    while (j < n && nums[j] == nums[j - 1]) j++;
                    while (k >= 0 && nums[k] == nums[k + 1]) k--;
                } else if (sum2 < sum) {
                    j++;
                } else if (sum2 > sum) {
                    k--;
                }
            }
        }

        return res;
    }
}
```
Time complexity - O(n^2), sorting costs O(n log n) and the two-pointer scan dominates
Space complexity - O(1) auxiliary space (excluding the output list)
