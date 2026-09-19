Prob: https://leetcode.com/problems/trapping-rain-water/description/

Sol 1: Optimal - Using two pointers
Intuition - We have to find max water that can be stored in a container. Take a two pointer such that left represents the left side of container and right represents the right side. Also the height of the container will the min of height at left and right
take a water and store it. Now as we have to find max water then move pointer which has less height and in this way we will calculate
the max water that can be stored in the container.
```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int left_index = 0;
        int right_index = n - 1;
        int left_max = 0; // to store left most max bar height
        int right_max = 0; // to store right most max bar height
        int total = 0;

        while (left_index <= right_index) {
            // Check which bar / building is smaller for min water trap
            if (height[left_index] < height[right_index]) {
                if (left_max > height[left_index]) {
                    total += left_max - height[left_index];
                } else
                    left_max = height[left_index];
                left_index++;
            } else {
                if (right_max > height[right_index]) {
                    total += right_max - height[right_index];
                } else
                    right_max = height[right_index];
                right_index--;

            }
        }
        return total;
    }

}
```
Time complexity - O(n),
Space complexity - O(1)