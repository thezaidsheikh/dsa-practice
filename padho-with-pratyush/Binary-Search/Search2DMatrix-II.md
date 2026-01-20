Prob: https://leetcode.com/problems/search-a-2d-matrix-ii/description/

Sol 1: Optimal Solution - Using binary Search with 2 pointer
1. Take two pointers for row and column.
2. Starting with col = 0 and row = length,
3. If elem at that row and column is greater then the targer, it is obvious that the other elem of the row is also greater as it is a sorted matrix.
4. Eliminate that row by decrementing the row by 1.
5. If elem at that row and column is lesser then the target, it is obvious that the other elem in the coloumn is also less.
6. Eliminate the col by increasing the col by 1.
7. In this way we can find the target elem.

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int n = matrix.length;
        int m = matrix[0].length;
        int row = n - 1, col = 0;

        while(row >= 0 && col < m) {
            int elem = matrix[row][col];
            if(elem == target) return true;
            if(elem > target) row--;
            else col++;
        }
        return false;
    }
}
```
Time Complexity - O(M+N)
Space Complexity - O(1)