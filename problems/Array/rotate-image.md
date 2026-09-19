Prob: https://leetcode.com/problems/rotate-image/description/

Sol 1: Brute force - Using extra space
1. Initialize an empty matrix of the same size (n x n).
2. Loop through every element of the original matrix using two nested loops.
3. For each element at position (i, j), place it in the new matrix at position (j, n - i - 1).
4. After completing the placement for all elements, return or copy the new matrix.
```java
class Solution {
    // Function to rotate the matrix 90 degrees clockwise using extra space
    public int[][] rotateClockwise(int[][] matrix) {
        // Get the size of the square matrix
        int n = matrix.length;

        // Create a new matrix of same size to store rotated result
        int[][] rotated = new int[n][n];

        // Traverse each element of original matrix
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                // Place the element at its new rotated position
                rotated[j][n - i - 1] = matrix[i][j];
            }
        }

        // Return the rotated matrix
        return rotated;
    }
}
```
Time complexity = O(N^2)
Space complexity = O(N^2)

Sol 2: Optimal - Using Constant space
1. Transposing moves elements from (i, j) to (j, i), effectively rotating across the diagonal.
2. Reversing the rows repositions the elements in each row, finalizing the clockwise rotation.
3. Get the size of the square matrix (number of rows or columns).
4. Start the first phase: Transpose the matrix
5. For each row starting from the top to bottom:
5. For each column starting from the diagonal element (excluding already visited elements):
6. Swap the current element with the element that is diagonally opposite across the main diagonal.
7. This effectively flips the matrix over its top-left to bottom-right diagonal, converting rows into columns.
8. Move to the second phase: Reverse each row
9. For every row in the matrix:
10. Reverse the order of the elements in that row (first element becomes last, second becomes second last, and so on).
11. This realigns the columns to their correct position for the clockwise rotation.
12. Once both phases are done, the matrix will have been rotated 90 degrees clockwise in-place.
```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        // Step 1: Transpose the matrix
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                // Swap element at (i, j) with (j, i)
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Step 2: Reverse each row
        for (int i = 0; i < n; i++) {
            int left = 0, right = n - 1;

            // Swap elements from both ends moving toward center
            while (left < right) {
                int temp = matrix[i][left];
                matrix[i][left] = matrix[i][right];
                matrix[i][right] = temp;
                left++;
                right--;
            }
        }
    }
}
```
Time complexity = O(N^2)
Space complexity = O(1)
