Prob: https://leetcode.com/problems/sort-colors/description/

Sol 1: Brute Force - Using sort method
1. Sort the array
2. Return the sorted array
```java
class Solution {
    public void sortColors(int[] nums) {
        Arrays.sort(nums);
    }
}
```
Time Complexity: O(nlogn),
Space Complexity: O(1)

Sol 2: Optimal - Using Dutch National Flag Algorithm

2. The Dutch National Flag Algorithm
- The idea is to sort the array into three parts: less than pivot, equal to pivot, greater than pivot
- Three pointers are used: left, mid, and right
- left pointer is for less than pivot, mid pointer is for equal to pivot, right pointer is for greater than pivot
- We iterate through the array and based on the value of mid pointer, we increment left or right pointer
```java
class Solution {
    public void sortColors(int[] nums) {
        int n = nums.length;
        int low = 0;
        int mid = 0;
        int high = n - 1;

        while(mid <= high) {
            if(nums[mid] == 0) {
                int temp = nums[mid];
                nums[mid] = nums[low];
                nums[low] = temp;
                mid ++;
                low ++;
            } else if(nums[mid] == 1) mid++;
            else if(nums[mid] == 2) {
                int temp = nums[mid];
                nums[mid] = nums[high];
                nums[high] = temp;
                high --;
            }
        }
    }
}
```
Time Complexity: O(n),
Space Complexity: O(1)