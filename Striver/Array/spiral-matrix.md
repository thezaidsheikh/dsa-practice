Prob: https://leetcode.com/problems/spiral-matrix/description/

Sol 1: Optimal - Using loop
The brute force method simulates movement in four directions: right, down, left, and up while keeping track of which cells have already been visited using a separate matrix. This approach ensures that we never revisit any element and always stay within bounds. After moving in one direction as far as possible, we rotate the direction and keep repeating until all elements are visited.
Initialize a 2D boolean matrix `visited` of same size as input to keep track of visited cells.
1. Define direction arrays for right, down, left, and up movements.
2. Start at (0, 0), and begin with direction = 0 (right).
3. For each of the total elements in the matrix:
4. Add the current cell to result and mark it as visited.
5. Check if the next cell in the current direction is valid and not visited.
6. If valid, move to it; else rotate the direction and try the new direction.
7. Repeat this for total number of cells in the matrix.
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> ls = new ArrayList<>();
        int n = matrix.length;
        int m = matrix[0].length;
        int left = 0, right = m - 1;
        int top = 0, bottom = n - 1;

        while (left <= right && top <= bottom) {
            // right move
            for (int i = left; i <= right; i++) {
                ls.add(matrix[top][i]);
            }
            top++;

            // bottom move
            for (int i = top; i <= bottom; i++) {
                ls.add(matrix[i][right]);
            }
            right--;

            if (top <= bottom) {
                // left move
                for (int i = right; i >= left; i--) {
                    ls.add(matrix[bottom][i]);
                }
                bottom--;
            }

            if (left <= right) {
                // top move
                for (int i = bottom; i >= top; i--) {
                    ls.add(matrix[i][left]);
                }
                left++;
            }
        }
        return ls;
    }
}
```
Time: O(n * m)
Space: O(n * m)