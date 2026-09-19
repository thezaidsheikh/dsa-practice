Prob: https://leetcode.com/problems/sort-colors/description/

Sol 1: Brute Force - Count and rewrite
1. Count how many 0s, 1s, and 2s are present in the array.
2. Rewrite the array in sorted order using the counted frequencies.

```java
class Solution {
    public void sortColors(int[] nums) {
        int zero = 0, one = 0, two = 0;
        for (int num : nums) {
            if (num == 0) zero++;
            else if (num == 1) one++;
            else two++;
        }

        int index = 0;
        while (zero-- > 0) nums[index++] = 0;
        while (one-- > 0) nums[index++] = 1;
        while (two-- > 0) nums[index++] = 2;
    }
}
```
Time complexity - O(n)
Space complexity - O(1)

Sol 2: Better - Two passes with partitioning
1. Move all 0s to the front in one pass.
2. Then move all 2s to the back in a second pass.
3. The remaining elements in the middle will be 1s.

```java
class Solution {
    public void sortColors(int[] nums) {
        int left = 0;
        int right = nums.length - 1;

        for (int i = 0; i <= right; i++) {
            if (nums[i] == 0) {
                int temp = nums[i];
                nums[i] = nums[left];
                nums[left] = temp;
                left++;
            }
        }

        for (int i = nums.length - 1; i >= left; i--) {
            if (nums[i] == 2) {
                int temp = nums[i];
                nums[i] = nums[right];
                nums[right] = temp;
                right--;
            }
        }
    }
}
```
Time complexity - O(n)
Space complexity - O(1)

Sol 3: Optimal - Dutch National Flag
# Intuition
Because the array contains only 0s, 1s, and 2s, we can maintain three regions in one pass: values before `low` are 0s, values between `low` and `mid` are 1s, and values after `high` are 2s. Each swap grows one region without ever reprocessing already-fixed elements.

1. Keep `low`, `mid`, and `high` pointers.
2. If `nums[mid]` is 0, swap it with `nums[low]` and advance both `low` and `mid`.
3. If `nums[mid]` is 1, just advance `mid`.
4. If `nums[mid]` is 2, swap it with `nums[high]` and decrease `high`.

```java
class Solution {
    public void sortColors(int[] nums) {
        int n = nums.length;
        int low = 0;
        int mid = 0;
        int high = n - 1;

        while (mid <= high) {
            if (nums[mid] == 0) {
                int temp = nums[mid];
                nums[mid] = nums[low];
                nums[low] = temp;
                mid++;
                low++;
            } else if (nums[mid] == 1) {
                mid++;
            } else {
                int temp = nums[mid];
                nums[mid] = nums[high];
                nums[high] = temp;
                high--;
            }
        }
    }
}
```
Time complexity - O(n), single pass
Space complexity - O(1), in-place
