Prob: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/description/

Sol 1: Optimal Solution - Using binary search
1. Assume we have a number line and in this we have to find smallest element at the kth number.
2. So the numbers which are left to kth number are less than it and number which are on right are bigger.
3. Now we will guess a number that can be the elem which is smaller
4. Also now we will find the total count that is smaller then our guessed number.
5. If the count is higher or equal to target or kth number then it is obvious that this number can be the smallest.
6. If the count is lesser than it is obvious that to find the kth number we have to find at least k count.
7. In this way our guessed number will bw the anser.

```java
class Solution {
    public static int fun(int[][] mat, int n, int m, int guess) {
        int row = n - 1;
        int col = 0;
        int count = 0;
        while(row >= 0 && col < m) {
            if(mat[row][col] <= guess) {
                count += row + 1;
                col++;
            } else {
                row--;
            }
        }
        return count;
    }
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int m = matrix[0].length;
        int low = matrix[0][0];
        int high = matrix[n-1][m-1];
        int res = -1;
        while(low <= high) {
            int guess = (low + high) / 2;
            int ans = fun(matrix,n,m,guess);
            if(ans < k) low = guess + 1;
            else {
                res = guess;
                high = guess - 1;
            }
        }
        return res;
    }
}
```
Time complexity - O(n + m) (for function) + O(log R) = O(n * log R),
Space complexity - O(1)