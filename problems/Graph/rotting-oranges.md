Prob: https://leetcode.com/problems/rotting-oranges/description/

Sol 1: Using BFS (Multi-Source BFS)

Solution Explanation:

1. Count all fresh oranges and push every initially rotten orange into a queue.
2. Treat all rotten oranges as starting points because rotting spreads from all of them at the same time.
3. Process the queue level by level. One full level means one minute has passed.
4. For each rotten orange, check its 4 directions: up, right, down, and left.
5. If a neighboring cell is a fresh orange, rot it, reduce the fresh count, and push it into the queue.
6. Continue until either all fresh oranges become rotten or the queue becomes empty.
7. If fresh oranges are still left at the end, return `-1`. Otherwise return the total time.

Intuition: This is a shortest-spread problem on a grid, so BFS fits naturally. Since multiple rotten oranges can start spreading at the same time, we use multi-source BFS by inserting all rotten oranges into the queue initially. Each BFS layer represents one minute of spreading.

```java
class Solution {
    private static int row = 0;
    private static int col = 0;

    public static boolean isValid(int i, int j) {
        if(i < 0 || i >= row) return false;
        else if(j < 0 || j >= col) return false;
        else return true;
    }

    public int orangesRotting(int[][] grid) {
        int fresh = 0;
        Queue<int[]> queue = new LinkedList<>();
        int time = 0;
        row = grid.length;
        col = grid[0].length;

        for(int i = 0; i < row; i++) {
            for(int j = 0; j < col; j++) {
                if(grid[i][j] == 1) fresh++;
                else if(grid[i][j] == 2) {
                    grid[i][j] = -1;
                    queue.offer(new int[]{i, j});
                }
            }
        }

        while(!queue.isEmpty() && fresh > 0) {
            time++;
            int size = queue.size();

            while(size-- > 0) {
                int[] top = queue.poll();
                int i = top[0];
                int j = top[1];

                // top
                if(isValid(i - 1, j) && grid[i - 1][j] == 1) {
                    fresh--;
                    grid[i - 1][j] = 0;
                    queue.offer(new int[]{i - 1, j});
                }

                // right
                if(isValid(i, j + 1) && grid[i][j + 1] == 1) {
                    fresh--;
                    grid[i][j + 1] = 0;
                    queue.offer(new int[]{i, j + 1});
                }

                // bottom
                if(isValid(i + 1, j) && grid[i + 1][j] == 1) {
                    fresh--;
                    grid[i + 1][j] = 0;
                    queue.offer(new int[]{i + 1, j});
                }

                // left
                if(isValid(i, j - 1) && grid[i][j - 1] == 1) {
                    fresh--;
                    grid[i][j - 1] = 0;
                    queue.offer(new int[]{i, j - 1});
                }
            }
        }

        if(fresh > 0) return -1;
        return time;
    }
}
```

Time Complexity: O(m _ n) because every cell is visited at most once
Space Complexity: O(m _ n) in the worst case due to the queue
