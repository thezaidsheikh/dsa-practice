Prob: https://leetcode.com/problems/next-permutation/description/

Sol 1: Brute force
1. Take all permutation of the given array.
2. Permutations should be in the lexical / correct order.
3. Now the next permutation of the given array is the answer.

Time complexity = O(n!) (for generating all permutations) * O(n) (Size of array)
Space complexity = Should be huge as generating and storing permutation combinations is a huge task.

Sol 2: Optimal
1. Find the break point index where the next element is greater from current
2. If index is -1 then array is already sorted in descending order. Return the ascending order of the array
3. If index is present then swap the smallest element which is greater then element at index idx.
4. Now sort in descending order from idx to the array length

```java
class Solution {
    public static void sortDescending(int[] nums, int start, int end) {
        int left = start;
        int right = end;
        while (left < right) {
            int temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;
            left++;
            right--;
        }
    }
    public void nextPermutation(int[] nums) {
        int idx = -1;
        int n = nums.length;

        // Find the break point where the next element is greater from current
        for(int i = n-2; i >= 0; i--) {
            if(nums[i] < nums[i+1]) {
                idx = i;
                break;
            }
        }

        // If the array is already sorted in decreasing order
        if(idx == -1) {
            Arrays.sort(nums);
            return;
        }

        // Swap the smallest element which is greater then element at index idx
        for(int i = n-1; i > idx; i--) {
            if(nums[i] > nums[idx]) {
                int temp = nums[i];
                nums[i] = nums[idx];
                nums[idx] = temp;
                break;
            }
        }

        // Now sort in descending order from idx to the array length
        sortDescending(nums,idx+1, n-1);
    }
}
```
Time complexity = O(n) + O(n) = O(2n)
Space complexity = O(1)