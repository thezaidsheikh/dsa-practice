Prob: https://leetcode.com/problems/rearrange-array-elements-by-sign/description/

Sol 1: Using extra space taking arrays for positive and negative and answer
```java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        // Brute force
        int n = nums.length;
        List<Integer> pos = new LinkedList<>();
        List<Integer> neg = new LinkedList<>();

        for(int i = 0; i < nums.length; i++) {
            if(nums[i] < 0) neg.add(nums[i]);
            else pos.add(nums[i]);
        }

        int[] array = new int[n];
        for (int i = 0; i < n/2; i++) {
            array[i*2] = pos.get(i);
            array[(i*2)+1] = neg.get(i);
        }
        return array;
    }
}
```
Time complexity - O(n) + O(n/2) = O(n),
Space complexity - O(n) + O(n) + O(n) = O(n)

Sol 2: Using single loop with only new array
```java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        int posIndex = 0;
        int negIndex = 1;
        int n = nums.length;
        int[] ans = new int[n];
        for(int i = 0; i < n; i++) {
            if(nums[i] >= 0) {
                ans[posIndex] = nums[i];
                posIndex += 2;
            } else {
                ans[negIndex] = nums[i];
                negIndex += 2;
            }
        }
        return ans;
    }
}
```
Time complexity - O(n),
Space complexity - O(n)