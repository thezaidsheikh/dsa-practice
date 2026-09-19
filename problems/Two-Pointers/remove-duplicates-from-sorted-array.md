# Remove Duplicates from Sorted Array

Prob: https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/

Sol 1: Brute Force - Shifting elements on every duplicate
1. For each element, count how many duplicates follow it.
2. Shift the remaining elements left to overwrite duplicates.
3. Repeat until the end of the array, then return the new length.
4. This mutates the array many times, making it quadratic.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        int newLength = n;
        int i = 0;
        while (i < newLength - 1) {
            if (nums[i] == nums[i + 1]) {
                for (int j = i + 1; j < newLength - 1; j++) {
                    nums[j] = nums[j + 1];
                }
                newLength--;
            } else {
                i++;
            }
        }
        return newLength;
    }
}
```
Time complexity - O(n^2), shifting on every duplicate
Space complexity - O(1)

Sol 2: Better - Using an extra array
1. Since the array is sorted, equal elements are always adjacent.
2. Copy each element to a new array only if it differs from the previous one.
3. Copy the unique elements back into nums.
4. This is O(n) but needs O(n) extra space.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        int[] temp = new int[n];
        int index = 0;
        for (int i = 0; i < n; i++) {
            if (i == 0 || nums[i] != nums[i - 1]) {
                temp[index++] = nums[i];
            }
        }
        for (int i = 0; i < index; i++) {
            nums[i] = temp[i];
        }
        return index;
    }
}
```
Time complexity - O(n),
Space complexity - O(n)

Sol 3: Optimal - Using two pointers
# Intuition
The array is sorted, so every duplicate sits right next to its original. A slow pointer (low) marks where the next unique element should be placed, while a fast pointer (high) scans ahead. Whenever the fast pointer sees a new value, we place it right after the slow pointer. Each unique element is written exactly once, in place.

1. Initialize low at 0 (last known unique position) and high at 0.
2. Move high across the array.
3. If nums[high] != nums[low], a new unique element is found.
4. Store it at nums[low + 1] and advance low.
5. Continue until high reaches the end.
6. Return low + 1, the number of unique elements.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int length = nums.length;
        int low = 0;
        int high = 0;

        while (high < length) {
            if (nums[high] != nums[low]) {
                nums[low + 1] = nums[high];
                low++;
            }
            high++;
        }

        return low + 1;
    }
}
```
Time complexity - O(n), single pass with two pointers
Space complexity - O(1), in-place

Sol 4: Your Solution - Two pointers with swapping
1. Keep two pointers `i` and `j`, both starting at index 0.
2. Move `j` forward to scan the array.
3. Whenever `nums[i] != nums[j]`, swap `nums[j]` with `nums[i + 1]` and move `i` ahead.
4. Return `i + 1` as the count of unique elements.

```java
class Solution {
 public int removeDuplicates(int[] nums) {
 int len = nums.length;
 int i = 0;
 int j = 0;

 while(i < len && j < len) {
 if(nums[i] != nums[j]) {
 int temp = nums[j];
 nums[j] = nums[i+1];
 nums[i+1] = temp;
 i++;
 }
 j++;
 }

 return i+1;
 }
}
```
Time complexity - O(n), one pass
Space complexity - O(1)
