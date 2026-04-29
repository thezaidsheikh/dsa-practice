Prob: https://leetcode.com/problems/surrounded-regions/submissions/1991212422/

## Solution: DFS from Boundaries

### Solution Explanation

1. **Key Insight**: 'O' cells that cannot be captured are those connected to the boundary. Any 'O' reachable from the boundary stays 'O', others become 'X'.
2. **Algorithm**:
   - Start DFS from all 'O' cells on the four boundaries
   - Mark these reachable 'O' cells as '#' (temporary marker)
   - After processing, iterate through entire board:
     - 'O' → 'X' (not reachable from boundary, captured)
     - '#' → 'O' (was reachable, restore to 'O')

### Intuition

Think of it as: "Find all O's that escape capture by touching the border. Those are safe."

Instead of checking every 'O' and trying to see if it's captured (which would require complex flood-fill for each), we invert the problem:
- Start from boundary 'O's and mark everything they can reach
- Anything not marked is guaranteed to be captured

### Key Points

- Time: O(m × n) - each cell visited at most twice
- Space: O(m × n) for recursion stack in worst case
- Uses DFS to mark safe regions
- Boundary cells are the "escape routes"

```java
class Solution {
    public static void dfs(char[][] board, int row, int col, int row_length, int col_length) {
        board[row][col] = '#';

        // check top
        if(row > 0 && board[row - 1][col] == 'O') {
            dfs(board, row - 1, col, row_length, col_length);
        }
        // check bottom
        if(row < row_length - 1 && board[row + 1][col] == 'O') {
            dfs(board, row + 1, col, row_length, col_length);
        }
        // check left
        if(col < col_length - 1 && board[row][col + 1] == 'O') {
            dfs(board, row, col + 1, row_length, col_length);
        }
        // check right
        if(col > 0 && board[row][col - 1] == 'O') {
            dfs(board, row, col - 1, row_length, col_length);
        }
    }
    public void solve(char[][] board) {
        int row = 0, col = 0;
        int row_length = board.length;
        int col_length = board[0].length;

        // First Row
        for(col = 0; col < col_length; col++) {
            if(board[row][col] == 'O') {
                dfs(board, row, col, row_length, col_length);
            }
        }

        // last Row
        row = row_length - 1;
        for(col = 0; col < col_length; col++) {
            if(board[row][col] == 'O') {
                dfs(board, row, col, row_length, col_length);
            }
        }

        // First Col
        col = 0;
        for(row = 0; row < row_length; row++) {
            if(board[row][col] == 'O') {
                dfs(board, row, col, row_length, col_length);
            }
        }

        // Last Col
        col = col_length - 1;
        for(row = 0; row < row_length; row++) {
            if(board[row][col] == 'O') {
                dfs(board, row, col, row_length, col_length);
            }
        }

        for(int i = 0; i < row_length; i++) {
            for(int j = 0; j < col_length; j++) {
                if(board[i][j] == 'O') board[i][j] = 'X';
                else if(board[i][j] == '#') board[i][j] = 'O';
            }
        }
    }
}
```

Time Complexity: O(m × n)
Space Complexity: O(m × n)