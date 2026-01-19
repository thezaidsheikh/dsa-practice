Prob: https://leetcode.com/problems/search-a-2d-matrix/description/

Sol 1: Optimized Sol - Using 2 times binary search
1. Find the row in which the target element can be present.
2. Find the column in which the target element can be present.
3. Check if the target element is present in the matrix.
4. Return true if the target element is present in the matrix, else return false.
 ```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int n = matrix.length;
        int m = matrix[0].length;
        int low = 0, high = n-1;
        int target_row = -1;

        while(low <= high) {
            int guess_row = (low+high)/2;
            if(matrix[guess_row][0] <= target) {
                target_row = guess_row;
                low = guess_row + 1;
            } else {
                high = guess_row - 1;
            }
        }
        
        low = 0;
        high = m-1;

        if(target_row == -1) return false;

        while(low <= high) {
            int guess = (low + high) / 2;
            if(matrix[target_row][guess] == target) return true;
            else if(matrix[target_row][guess] > target) high = guess - 1;
            else low = guess + 1;
        }
        return false;
    }
}
```
Time complexity - O(logn)
Space complexity - O(1)

Sol 2: Optimized Sol - Using 1 times binary search
1. Treat the 2D matrix as a 1D array.
2. Use binary search to find the target element.
3. To check the number at a particular index in the 2D matrix, use the formula matrix[index/m][index%m].
4. Return true if the target element is present in the matrix, else return false.
 ```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int n = matrix.length;
        int m = matrix[0].length;
        int low = 0, high = n*m - 1;
        int target_row = -1;

        while(low <= high) {
            int guessIndex = (low + high) / 2;
            int row = guessIndex / m;
            int col = guessIndex % m;
            if(matrix[row][col] == target) return true;
            else if(matrix[row][col] > target) high = guessIndex - 1;
            else low = guessIndex + 1;
        }
        return false;
    }
}
```
Time complexity - O(logn)
Space complexity - O(1)