Prob: https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/description/

Sol 1: Brute Force - Build a new array with at most two copies
1. Traverse the array from left to right and track how many times the current value has appeared consecutively.
2. Write values into a temporary array only while frequency is at most 2.
3. Copy the temporary array back into `nums` and return the new length.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        int[] temp = new int[n];
        int idx = 0;
        int count = 0;
        int prev = Integer.MIN_VALUE;

        for (int num : nums) {
            if (num != prev) {
                prev = num;
                count = 1;
                temp[idx++] = num;
            } else if (count < 2) {
                temp[idx++] = num;
                count++;
            } else {
                count++;
            }
        }

        for (int i = 0; i < idx; i++) {
            nums[i] = temp[i];
        }
        return idx;
    }
}
```
Time complexity - O(n),
Space complexity - O(n)

Sol 2: Better - In-place write with boundary check
1. If array size is `<= 2`, answer is already the full length.
2. Keep a write pointer from index `2`.
3. For each next element, compare it with `nums[write - 2]`.
4. Write current element only when it differs from `nums[write - 2]`.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        if (n <= 2) return n;

        int write = 2;
        for (int read = 2; read < n; read++) {
            if (nums[read] != nums[write - 2]) {
                nums[write] = nums[read];
                write++;
            }
        }
        return write;
    }
}
```
Time complexity - O(n),
Space complexity - O(1)

Sol 3: Optimal - Using two pointers with duplicate count
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
