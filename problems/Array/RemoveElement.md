Prob: https://leetcode.com/problems/remove-element/description/?envType=study-plan-v2&envId=top-interview-150

Sol 1: Optimal Sol - 2 Pointers
1. Initialize index to 0, which represents the current position for the next non-target element.
2. Iterate through each element of the input array using the i pointer.
3. For each element nums[i], check if it is equal to the target value.
4. If nums[i] is not equal to val, it means it is a non-target element.
5. Set nums[index] to nums[i] to store the non-target element at the current index position.
6. Increment index by 1 to move to the next position for the next non-target element.
7. Continue this process until all elements in the array have been processed.
8. Finally, return the value of index, which represents the length of the modified array.

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int n = nums.length;
        int index = 0;

        for(int i = 0; i < n; i++) {
            if(nums[i] != val) {
                nums[index] = nums[i];
                index++;
            }
        }
        return index;
    }
}
```
Time complexity - O(n),
Space complexity - O(1)
