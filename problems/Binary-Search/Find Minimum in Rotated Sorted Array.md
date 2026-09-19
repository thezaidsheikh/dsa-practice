Prob: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/

Sol 1: Using linear search - normal for loop
```aiignore
class Solution {
    public int findMin(int[] nums) {
        int min = nums[0];
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] < min) {
                min = nums[i];
                break;
            };
        }
        return min;
    }
}
```
Time complexity - O(n),
Space complexity - O(1)

Sol 2: Using binary search algorithm
```aiignore
class Solution {
    public int findMin(int[] nums) {
        int n = nums.length;
        int low = 0;
        int high = n - 1;
        int min = Integer.MAX_VALUE;
        while(low <= high) {
            int guess = (low + high) / 2;
            if(nums[guess] > nums[n-1]) low = guess + 1;
            else {
                high = guess - 1;
                min = Math.min(min,nums[guess]);
            }
        }
        return min;
        
    }
}
```
Time complexity - O(log n),
Space complexity - O(1)