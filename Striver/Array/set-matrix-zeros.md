Prob: https://www.naukri.com/code360/problems/set-matrix-zeros_3846774

Sol 1: Brute Force - Using linear search 2 loops
1. Iterate ton row of the matrix.
2. Iterate to col of the matrix.
3. If any element at row and col is 0 then again iterate to that row and col and make all the element -1.
4. Iterate to the matrix and make all the element -1 to 0.
5. Return the matrix
```java
public class Solution {
    public static void setZeros(int[][] matrix) {
        int n = matrix.length;
        int m = matrix[0].length;

        // Step 1 & 2: iterate over all cells
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                // Step 3: if element is 0
                if (matrix[i][j] == 0) {

                    // mark entire row i as -1 (except already 0)
                    for (int col = 0; col < m; col++) {
                        if (matrix[i][col] != 0) {
                            matrix[i][col] = -1;
                        }
                    }

                    // mark entire column j as -1 (except already 0)
                    for (int row = 0; row < n; row++) {
                        if (matrix[row][j] != 0) {
                            matrix[row][j] = -1;
                        }
                    }
                }
            }
        }

        // Step 4: convert all -1 to 0
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == -1) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}

```
Time complexity - O((n×m)×(n+m)),
Space complexity - O(1)

Sol 2: Better approch - Using extra space
1. Use an array row[] of size n and col[] of size m initialized with 0.
2. First pass: whenever matrix[i][j] == 0, set row[i] = 1 and col[j] = 1.
3. Second pass: if row[i] == 1 or col[j] == 1, set matrix[i][j] = 0.
4. Return the matrix
```java
public class Solution {
    public static void setZeros(int[][] matrix) {
        int n = matrix.length;
        int m = matrix[0].length;

        int[] row = new int[n];
        int[] col = new int[m];

        // First pass: mark rows and columns that should be zero
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 0) {
                    row[i] = 1;
                    col[j] = 1;
                }
            }
        }

        // Second pass: set cells to zero based on row/col markers
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (row[i] == 1 || col[j] == 1) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```
Time complexity - O(n*m),
Space complexity - O(n + m)

Sol 3: Optimal approch - Using constant space
1. Key idea: those row and col arrays are just markers, so reuse the first row and first column of the matrix itself as these marker arrays.
2. Use first row to mark which columns become 0.
3. Use first column to mark which rows become 0.
4. Use a separate variable col0 to remember if the first column itself should be 0.
5. Cell (0,0) lies in both the first row and first column. So it cannot represent two things at once.
6. Use col0 to remember if the first column itself should be 0.
7. First pass: whenever matrix[i][j] == 0, set matrix[i][0] = 0 and matrix[0][j] = 0.
8. Second pass: if matrix[i][0] == 0 or matrix[0][j] == 0, set matrix[i][j] = 0.
9. Return the matrix
```java
class Solution {
    public void setZeroes(int[][] matrix) {

        int n = matrix.length;
        int m = matrix[0].length;

        // Variable to track if first column should be zero
        int col0 = 1;

        // ==============================
        // Step 1: Mark rows & columns
        // ==============================
        for (int i = 0; i < n; i++) {
            if (matrix[i][0] == 0) col0 = 0;

            for (int j = 1; j < m; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;   // mark row
                    matrix[0][j] = 0;   // mark column
                }
            }
        }

        // ==============================
        // Step 2: Fill inner matrix
        // ==============================
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }

        // ==============================
        // Step 3: First row
        // ==============================
        if (matrix[0][0] == 0) {
            for (int j = 0; j < m; j++) {
                matrix[0][j] = 0;
            }
        }

        // ==============================
        // Step 4: First column
        // ==============================
        if (col0 == 0) {
            for (int i = 0; i < n; i++) {
                matrix[i][0] = 0;
            }
        }
    }
}
```
Time complexity - O(n*m),
Space complexity - O(1)