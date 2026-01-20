Prob: https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/

Sol 1: Brute force - Using loop and array
1. Traverse the array and, for each element.
2. Linearly check a separate temp array to see if it’s already present before inserting it. 
3. Copy the temp array’s contents back to the start of input array.

```java
// Brute force: extra space, not in-place
class SolutionBrute {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0) return 0;

        int n = nums.length;
        int[] temp = new int[n];
        int k = 0;

        // Since nums is sorted, just copy when current != last unique
        temp[k++] = nums[0];
        for (int i = 1; i < n; i++) {
            if (nums[i] != temp[k - 1]) {
                temp[k] = nums[i];
                k++;
            }
        }

        // Copy uniques back to nums[0..k-1]
        for (int i = 0; i < k; i++) {
            nums[i] = temp[i];
        }

        return k;
    }
}

```
Time complexity - O(n),
Space complexity - O(n)

Sol 2: Better Sol - Using hashmap or set
1. In place of extra array use hashmap or set

```java
import java.util.LinkedHashSet;
import java.util.Set;

// Better: uses a Set (extra space), keeps order
class SolutionBetter {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0) return 0;

        Set<Integer> set = new LinkedHashSet<>();
        for (int num : nums) {
            set.add(num);
        }

        int k = 0;
        for (int val : set) {
            nums[k++] = val;
        }

        return k;
    }
}

```
Time complexity - O(n),
Space complexity - O(n)

Sol 3: Optimal Sol - Using 2 pointers
1. Maintain one pointer i for the position of the last unique element.
2. Second pointer j scanning forward.
3. Check if both elem are equal than it means it is duplicate then move j only by 1.
4. If not equal move then put the j elem in pos to i and move both pointers by 1.

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int i = 0;
        int j = 1;
        int n = nums.length;
        while(j < n && i < n) {
            if(nums[j] == nums[i]) j++;
            else if(nums[j] != nums[i]) {
                nums[i+1] = nums[j];
                i++;
                j++;
            }
        }

        return i+1;
    }
}
```
Time complexity - O(n),
Space complexity - O(1)