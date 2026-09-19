Prob: https://leetcode.com/problems/rotate-array

Sol 1: Brute force - Using extra array
1. Create a new array of size k.
2. Copy the last k elements from the original array to the new array.
3. Shifting the remaining elements to the right.
4. Assign the new array to the original array at start.
```java
class Solution {

    public void rotate(int[] nums, int k) {
        int n = nums.length;
        if (n <= 1) return;

        // Step 0: normalize k
        k = k % n;
        if (k == 0) return;

        // Step 1: Create a new array of size n (your step says k, but we need full size)
        int[] temp = new int[n];

        // Step 2: Copy the last k elements from the original array to the new array
        // nums: [1,2,3,4,5,6,7], k = 3  -> take [5,6,7] to the front
        for (int i = 0; i < k; i++) {
            temp[i] = nums[n - k + i];
        }

        // Step 3: Shift the remaining elements to the right
        // Copy the first n-k elements after the first k positions
        for (int i = 0; i < n - k; i++) {
            temp[k + i] = nums[i];
        }

        // Step 4: Assign the new array to the original array at start (copy back)
        for (int i = 0; i < n; i++) {
            nums[i] = temp[i];
        }
    }
}
```
Time complexity - O(n + k),
Space complexity - O(n)


Sol 2: Optimal - Using reverse
1. Reverse the first n-k elements.
2. Reverse the last k elements.
3. Reverse the entire array.
```java
class Solution {
    public static void reverse(int[] nums, int start, int end) {
        while(start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start ++;
            end --;
        }
    }
    public void rotate(int[] nums, int k) {
        if(k > nums.length) {
            if(k % nums.length == 0) return;
            else k = k % nums.length;
        }
        reverse(nums, 0, nums.length - k - 1);
        reverse(nums, nums.length - k, nums.length-1);
        reverse(nums, 0, nums.length -1);
    }
}
```
Time complexity - O(n),
Space complexity - O(1)
