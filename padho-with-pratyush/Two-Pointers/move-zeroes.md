Prob: https://leetcode.com/problems/move-zeroes/description/

Sol 1: Brute Force - Repeatedly shift non-zero values
1. Scan the array and whenever a zero is found, shift the following elements left by one.
2. Place zero at the end of the array.
3. Repeat until all zeroes are moved back.

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {
                for (int j = i; j < n - 1; j++) {
                    nums[j] = nums[j + 1];
                }
                nums[n - 1] = 0;
                i--;
                n--;
            }
        }
    }
}
```
Time complexity - O(n^2)
Space complexity - O(1)

Sol 2: Better - Two passes with extra write index
1. Keep a write pointer for the next non-zero element.
2. First copy all non-zero values to the front in order.
3. Fill the remaining positions with zeroes.

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int write = 0;
        for (int num : nums) {
            if (num != 0) {
                nums[write++] = num;
            }
        }
        while (write < nums.length) {
            nums[write++] = 0;
        }
    }
}
```
Time complexity - O(n)
Space complexity - O(1)

Sol 3: Optimal - Two pointers with stable in-place placement
# Intuition
Instead of searching for zeroes to swap, track the next position where a non-zero should go. Every non-zero is moved forward exactly once, and the relative order of non-zero values stays unchanged. Once all non-zero values are placed, fill the rest with zeroes.

1. Keep a write pointer `i` for the next non-zero position.
2. Scan the array with `j`.
3. Whenever `nums[j] != 0`, swap it into position `i` and advance `i`.
4. After the scan, positions from `i` to end are set to 0.

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int i = 0;
        for (int j = 0; j < nums.length; j++) {
            if (nums[j] != 0) {
                int temp = nums[j];
                nums[j] = nums[i];
                nums[i] = temp;
                i++;
            }
        }
    }
}
```
Time complexity - O(n), single pass
Space complexity - O(1), in-place
