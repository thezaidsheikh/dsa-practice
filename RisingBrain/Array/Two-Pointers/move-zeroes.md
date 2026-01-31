Prob: https://leetcode.com/problems/move-zeroes/

Sol 1: Brute Force - Using extra space
1. Create a new array of size N
2. Iterate over the input array
3. If the element is non-zero, copy it to the new array
4. Return the new array
```java
class SolutionBruteForce {
    public void moveZeroes(int[] nums) {
        int n = nums.length;
        int index = 0;
        for (int num : nums) {
            if (num != 0) {
                nums[index++] = num;
            }
        }
        while (index < n) {
            nums[index++] = 0;
        }
    }
}
```
Time Complexity: O(n),
Space Complexity: O(1)

Sol 2: Optimal - Using Two Pointers
1. Initialize two pointers, left and right
2. Iterate over the input array
3. If the element is non-zero, swap it with the element at the left pointer
4. Increment the left pointer
5. Return the input array
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length;
        int i = 0;
        int j = 1;

        while(i < n && j< n) {
            if(nums[i] == 0 && nums[j] != 0) {
                int temp = nums[j];
                nums[j] = nums[i];
                nums[i] = temp;
            } else if(nums[i] == 0 && nums[j] == 0) {
                j++;
                continue;
            }
            i++;
            j++;
        }
    }
}
```
Time Complexity: O(n),
Space Complexity: O(1)
