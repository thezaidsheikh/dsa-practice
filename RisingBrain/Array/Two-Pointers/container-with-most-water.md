Prob: https://leetcode.com/problems/container-with-most-water/description/

Sol 1: Optimal - Using Two pointers
Intitution - We have to find max container for which the volume should be maximum. Take a two pointer such that i represent
the left side of container and j represent the right side. Also the length of the container will the min of height at i and j
take a vol and store it. Now as we have to find max vol then move pointer which has less height and in this way we will calculate
the max volume.
1. Take 2 pointers i and j
2. Start i = 0 and j = n - 1 and take a max var to store max vol
3. Calculate min height of the container and vol check the max each time
4. Move pointers according to min height at i or j.
```java
class Solution {
    public int maxArea(int[] height) {
        int n = height.length;
        int low = 0;
        int high = n - 1;
        int max = Integer.MIN_VALUE;

        while(low <= high) {
            int breadth = high - low;
            int minLength = Math.min(height[low],height[high]);
            int vol = breadth * minLength;
            max = Math.max(max,vol);
            if(height[low] <= height[high]) low++;
            else high--;
        }
        return max;
    }
}
```