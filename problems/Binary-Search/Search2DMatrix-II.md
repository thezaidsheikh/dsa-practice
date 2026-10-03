Prob: https://leetcode.com/problems/search-a-2d-matrix-ii/description/

Sol 1: Optimal Solution - Using binary Search with 2 pointer
1. Take two pointers for row and column.
2. Start at the bottom-left corner: row = n - 1, col = 0.
3. If the element at that position equals the target, return true.
4. If the element is greater than the target, the whole row to its left is also greater (rows are sorted ascending), so eliminate that row by decrementing row.
5. If the element is lesser than the target, the whole column above it is also lesser (columns are sorted ascending), so eliminate that column by incrementing col.
6. Each step removes one row or one column, so we never revisit a position.
7. Loop while row >= 0 and col < m, then return false.

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
Time Complexity - O(M+N),
Space Complexity - O(1)