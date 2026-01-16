Prob: https://leetcode.com/problems/merge-sorted-array/description/?envType=study-plan-v2&envId=top-interview-150

Sol 1: Usign extra space for storing the merged sorted array
1. Create a new array of size m + n.
2. Copy the elements of nums1 and nums2 into the new array.
3. Sort the new array.
4. Copy the elements of the new array back into nums1.

Time complexity = O(m + n) + O(m + n) + O(m + n) = O(m + n)
Space complexity = O(m + n)

Sol 2: Using two pointers
1. Start from the end of the array.
2. Compare the elements of nums1 and nums2.
3. Copy the greater element into nums1.
4. Move the pointer of the greater element to the left.
5. Repeat steps 2-4 until all elements of nums2 are copied into nums1.
 ```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int last = (m+n) - 1;
        while(m > 0 && n > 0) {
            if(nums1[m - 1] > nums2[n - 1]) {
                nums1[last] = nums1[m - 1];
                m -= 1;
            } else {
                nums1[last] = nums2[n - 1];
                n -= 1;
            }
            last -= 1;
        }

        while(n > 0) {
            nums1[last] = nums2[n - 1];
            n -= 1;
            last -= 1;
        }
    }
}
```
Time complexity = O(m + n)
Space complexity = O(1)