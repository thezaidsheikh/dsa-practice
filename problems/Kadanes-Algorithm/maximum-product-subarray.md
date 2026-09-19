Prob: https://leetcode.com/problems/maximum-product-subarray/

Sol 1: Brute Force - Checking all subarrays
1. Run a loop (i) that selects every possible starting index of the subarray.
2. Run a loop (j) that signifies the ending index of the subarray.
3. Calculate the product of each subarray and track the maximum product seen so far.
4. Return the maximum product.

```java
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int max = Integer.MIN_VALUE;

        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int prod = 1;
                for (int m = i; m <= j; m++) {
                    prod *= nums[m];
                }
                max = Math.max(max, prod);
            }
        }
        return max;
    }
}
```
Time Complexity: O(n^3), because for every pair of start and end indices we recompute the subarray product from scratch.
Space Complexity: O(1)

Sol 2: Better - Building the product incrementally
1. Notice that the product of the subarray ending at j can be built incrementally from the subarray ending at j-1.
2. As j moves forward, multiply nums[j] into the running product instead of recomputing from scratch.
3. Track the maximum running product across all subarrays.

```java
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int max = Integer.MIN_VALUE;

        for (int i = 0; i < n; i++) {
            int prod = 1;
            for (int j = i; j < n; j++) {
                prod *= nums[j];
                max = Math.max(max, prod);
            }
        }
        return max;
    }
}
```
Time Complexity: O(n^2), for each start index we extend the subarray while maintaining a running product.
Space Complexity: O(1)

Sol 3: Optimal - Modified Kadane's algorithm tracking both max and min products
# Intuition
Kadane's trick of dropping a negative prefix fails here because multiplication behaves differently from addition: a very negative product can instantly become the maximum after multiplying one more negative number. So for every index i we must track BOTH the maximum and the minimum product of a subarray ending exactly at i — the minimum is valuable because it may flip into the future maximum. When nums[i] is negative, multiplying by it swaps which of prevMax/prevMin is the better extension, so we swap them first and then apply the usual Kadane decision: extend the previous subarray or restart fresh at nums[i].

1. Initialize prevMax = 1 and prevMin = 1 (neutral identity for products) and res = Integer.MIN_VALUE.
2. Handle n == 1 separately by returning nums[0].
3. For each element, if nums[i] < 0, swap prevMax and prevMin so the roles stay correct after the sign flip.
4. Update prevMax = max(nums[i], nums[i] * prevMax): either start a new subarray or extend the best one.
5. Update prevMin = min(nums[i], nums[i] * prevMin): same decision, keeping the most negative product alive.
6. Track res as the maximum of all prevMax values.

```java
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int prevMax = 1;
        int prevMin = 1;
        int res = Integer.MIN_VALUE;

        if(n == 1) return nums[0];

        for(int i = 0; i < n; i++) {
            if(nums[i] < 0) {
                int tmp = prevMax;
                prevMax = prevMin;
                prevMin = tmp;
            }
            prevMax = Math.max(nums[i], nums[i] * prevMax);
            prevMin = Math.min(nums[i], nums[i] * prevMin);
            res = Math.max(res, prevMax);
        }

        return res;
    }
}
```
Time Complexity: O(n), single pass over the array.
Space Complexity: O(1)

## Key Takeaways
- Products break standard Kadane's logic: negatives make small values valuable, so the DP state must carry both extremes (max AND min product ending here).
- The swap-on-negative is just bookkeeping — equivalently you can evaluate three candidates per element without swapping: max(nums[i], nums[i]*prevMax, nums[i]*prevMin).
- Recognition cue: "maximum product of a contiguous subarray" -> Kadane's DP state ("best ending here") but doubled into min/max states.
- Sibling of Maximum Subarray in this folder — same "extend or restart" skeleton, upgraded with the min-max state trick.
