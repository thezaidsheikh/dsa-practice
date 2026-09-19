Prob: https://leetcode.com/problems/number-of-islands/description/

Sol 1: DFS (Depth First Search)

Solution Explanation:

1. Iterate through each cell in the grid.
2. When a '1' (land) is found:
   - Increment island count.
   - Run DFS to mark the entire connected island as visited (change to '*').
3. DFS marks current cell as visited, then recursively visits all 4 directions (up, down, left, right).
4. Continue until all connected land cells are marked as visited.
5. Return total count.

Intuition: Each '1' that hasn't been visited represents a new island. When found, we perform DFS/BFS to visit all connected land cells and mark them as visited. This ensures each island is counted exactly once. The 2D grid can be treated as a graph where each cell is a node and 4-directional neighbors are edges.

```java
class Solution {
    private static int[] neighbourRowArr = {-1, 1, 0, 0};
    private static int[] neighbourColArr = {0, 0, -1, 1};
    private static int m = 0;
    private static int n = 0;
    private static int res = 0;

    private static boolean isValid(int row, int col) {
        if(row < 0 || row >= m) return false;
        else if(col < 0 || col >= n) return false;
        return true;
    }

    public static void dfs(char[][] grid, int row, int col) {
        grid[row][col] = '*';
        for(int i = 0; i < neighbourRowArr.length; i++) {
            int neighbourRow = row + neighbourRowArr[i];
            int neighbourCol = col + neighbourColArr[i];
            if(isValid(neighbourRow, neighbourCol) && grid[neighbourRow][neighbourCol] == '1') {
                dfs(grid, neighbourRow, neighbourCol);
            }
        }
        return;
    }
    
    public int numIslands(char[][] grid) {
        m = grid.length;
        n = grid[0].length;
        res = 0;

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(isValid(i, j) && grid[i][j] == '1') {
                    dfs(grid, i, j);
                    res++;
                }
            }
        }
        return res;
    }
}
```

Time Complexity: O(m * n) - each cell visited at most once
Space Complexity: O(m * n) in worst case for recursion stack