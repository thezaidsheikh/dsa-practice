Prob: https://www.geeksforgeeks.org/problems/aggressive-cows/1

Sol 1: Using Binary Search
1. Sort the stalls array.
2. Generate guess number using binary search.
3. Check if the shed distance is greater or equal to the guess number.
4. Check for all cows if they can be placed at the guess number distance.
5. If yes, then store the guess number in the answer and search for a greater guess number.
6. If no, then search for a smaller guess number.
7. Return the answer.

```java
class Solution {
    public static boolean isPossible(int[] stalls, int n, int k, int guess) {
        int cows = 1;
        int pos = stalls[0];
        for(int i = 1; i < n; i++) {
            int diff = stalls[i] - pos;
            if(diff >= guess) {
                 cows++;
                 pos = stalls[i];
            }
        }
        if(cows < k) return false;
        return true;
    }
    public int aggressiveCows(int[] stalls, int k) {
        Arrays.sort(stalls);
        
        int n = stalls.length;
        int low = 1, high = stalls[n-1] - stalls[0];
        int res = -1;
        
        while(low <= high) {
            int guess = (low + high) / 2;
            if(isPossible(stalls, n, k, guess)) {
                res = Math.max(guess,res);
                low = guess + 1;
            } else {
                high = guess - 1;
            }
        }
        return res;
        
    }
}
```
Time complexity = O(nlogm) + O(nlogm),
Space complexity = O(1)
