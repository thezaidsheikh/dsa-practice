Prob: https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/

Sol 1: Optimal - Using two pointers
1. Initialize two pointers, one at the start and one at the end of the array.
2. If the sum of the elements at the two pointers is less than the target, increment the left pointer.
3. If the sum of the elements at the two pointers is greater than the target, decrement the right pointer.
4. If the sum of the elements at the two pointers is equal to the target, return the indices of the two elements.
```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;

        while (left < right) {
            int sum = numbers[left] + numbers[right];

            if (sum == target) {
                return new int[] { left + 1, right + 1 }; // 1-indexed
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }

        return new int[0];
    }
}
```
Time complexity - O(n),
Space complexity - O(1)