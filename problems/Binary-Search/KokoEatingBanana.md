Prob: https://leetcode.com/problems/koko-eating-bananas/description/

Sol 1: Using Binary Search with guessing the ans
1. Sort the array
2. Find the max element in the array
3. Guess the ans till max
4. Check if the ans is possible
5. If yes, then store the ans and search for a smaller ans
6. If no, then search for a greater ans
7. Return the ans

```java
class Solution {
    public static boolean isBananaEaten(int[] piles, int h, int n, int guess) {
        int hour = 0;
        for(int i = 0; i < n; i++) {
            int ans = piles[i] / guess;
            hour += ans;
            if(piles[i] % guess != 0) hour++;
            if(hour > h) return false;
        }
        return true;
    }
    public int minEatingSpeed(int[] piles, int h) {
        Arrays.sort(piles);
        int n = piles.length;
        int low = 1, high = piles[n-1];
        int res = -1;
        while(low <= high) {
            int guess = (low + high) / 2;
            if(isBananaEaten(piles,h,n,guess)) {
                res = guess;
                high = guess - 1;
            } else {
                low = guess + 1;
            }
        }
        return res;
    }
}
```
Time complexity = O(nlogm) + O(nlogm),
Space complexity = O(1)