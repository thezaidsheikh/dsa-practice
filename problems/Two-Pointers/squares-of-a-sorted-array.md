# Squares of a Sorted Array

Prob: https://leetcode.com/problems/squares-of-a-sorted-array/description/

Sol 1: Brute Force - Square and sort
1. Square every element in the given array.
2. Sort the whole array after squaring.
3. Return the sorted squared values.

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            nums[i] = nums[i] * nums[i];
        }
        Arrays.sort(nums);
        return nums;
    }
}
```
Time complexity - O(n log n),
Space complexity - O(1) extra (ignoring sort internals)

Sol 2: Better - Split negatives/positives, then merge (your approach)
1. Traverse the array and store squared negatives in one list and squared non-negatives in another.
2. Reverse the negatives list because larger absolute negatives appear first in input, so after squaring they must be read in reverse order.
3. Merge the two sorted lists like merge step of merge sort.
4. Convert the merged list to an int array and return it.

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        List<Integer> neg = new ArrayList<>();
        List<Integer> pos = new ArrayList<>();
        List<Integer> res = new ArrayList<>();

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] < 0) neg.add(nums[i] * nums[i]);
            else pos.add(nums[i] * nums[i]);
        }

        if (neg.size() == 0) return pos.stream().mapToInt(Integer::intValue).toArray();
        else Collections.reverse(neg);

        if (pos.size() == 0) return neg.stream().mapToInt(Integer::intValue).toArray();

        int i = 0;
        int j = 0;
        int n1 = neg.size();
        int n2 = pos.size();

        while (i < n1 && j < n2) {
            if (neg.get(i) < pos.get(j)) {
                res.add(neg.get(i));
                i++;
            } else if (neg.get(i) > pos.get(j)) {
                res.add(pos.get(j));
                j++;
            } else {
                res.add(pos.get(j));
                res.add(neg.get(i));
                i++;
                j++;
            }
        }

        while (i < n1) {
            res.add(neg.get(i));
            i++;
        }

        while (j < n2) {
            res.add(pos.get(j));
            j++;
        }

        return res.stream().mapToInt(Integer::intValue).toArray();
    }
}
```
Time complexity - O(n), one pass + reverse + merge
Space complexity - O(n), extra lists for neg/pos/result

Sol 3: Optimal - Two pointers from both ends
# Intuition
The largest square always comes from the number with the largest absolute value, which will be at either end of the sorted array. Compare both ends, place the larger square at the end of the result array, and move inward.

1. Keep `left` at start and `right` at end.
2. Create a result array and fill it from the last index to first.
3. Compare `abs(nums[left])` and `abs(nums[right])`.
4. Put the larger square at current result index and move that pointer.
5. Continue until all positions are filled.

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        int left = 0, right = n - 1;

        for (int i = n - 1; i >= 0; i--) {
            int leftVal = nums[left] * nums[left];
            int rightVal = nums[right] * nums[right];

            if (leftVal > rightVal) {
                ans[i] = leftVal;
                left++;
            } else {
                ans[i] = rightVal;
                right--;
            }
        }

        return ans;
    }
}
```
Time complexity - O(n), single pass
Space complexity - O(n), output array
