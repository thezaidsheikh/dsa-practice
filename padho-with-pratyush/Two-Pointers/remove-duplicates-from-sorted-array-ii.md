# Remove Duplicates from Sorted Array II

Prob: https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/description/

Sol 1: Optimal - Using two pointers with duplicate count
# Intuition
Since the array is sorted, duplicates come together. Keep `i` as the last valid index in the compacted part and `j` as the scanner. Track how many times the current value has appeared. Allow writing only up to 2 copies.

1. Start with `i = 0`, `j = 1`, and `count = 1`.
2. If `nums[i] == nums[j]`, increase count.
3. Only when count becomes 2, place this second occurrence at `nums[i + 1]` and move `i`.
4. If a new value appears, write it at `nums[i + 1]`, reset count to 1, and move `i`.
5. Continue until `j` reaches the end, then return `i + 1`.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int len = nums.length;
        int count = 1;
        int i = 0;
        int j = 1;

        while (i < len && j < len) {
            if (nums[i] == nums[j]) {
                count++;
                if (count == 2) {
                    nums[i + 1] = nums[j];
                    i++;
                }
                j++;
            } else {
                nums[i + 1] = nums[j];
                i++;
                count = 1;
                j++;
            }
        }

        return i + 1;
    }
}
```

Time complexity - O(n), single pass
Space complexity - O(1), in-place
