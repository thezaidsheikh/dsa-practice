Prob: https://leetcode.com/problems/3sum/description/

Sol 1: Brute Force - Using 3 nested loops
1. For each element in array, fix it as `A[i]`
2. For each remaining element in array, fix it as `A[j]`
3. For each remaining element in array, fix it as `A[k]`
4. If `A[i] + A[j] + A[k] == 0` then `A[i], A[j], A[k]` is a triplet
5. If `A[i] + A[j] + A[k] < 0` then increment `j`
6. If `A[i] + A[j] + A[k] > 0` then decrement `k`
7. Return all the triplets
```java
class SolutionBruteForce {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        Set<List<Integer>> set = new HashSet<>();

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> triplet =
                                Arrays.asList(nums[i], nums[j], nums[k]);
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
Time Complexity: O(n^3)
Space Complexity: O(k)

Sol 2: Optimal - Using Three Pointers
1. Intuition - Sort the array and use two pointers to find the triplets
2. First pointer is fixed and second and third pointer are moved
3. If I have i = 4 then j+k should be equal to -4 to make the sum 0
4. Using this intuition, we can use two pointers to find the triplets
```java
class SolutionOptimized {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();

        int n = nums.length;

        for (int i = 0; i < n; i++) {

            if (i > 0 && nums[i] == nums[i - 1]) continue;

            int left = i + 1;
            int right = n - 1;

            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];

                if (sum == 0) {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));

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

        return result;
    }
}
```
Time Complexity: O(n^2), // n is the length of the array
Space Complexity: O(k) // k is the number of triplets and use for result only
