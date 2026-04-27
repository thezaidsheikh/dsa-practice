Prob: https://leetcode.com/problems/is-graph-bipartite/description/

## Solution: DFS Coloring

### Solution Explanation

1. Maintain a `coloredNodes` array where:
   - `-1` = unvisited/uncolored
   - `0` = color A
   - `1` = color B
2. For each unvisited node, start DFS with color 0
3. In DFS:
   - Color the current node
   - For each neighbor:
     - If neighbor has same color → NOT bipartite, return false
     - If neighbor is uncolored → recursively color with opposite color (1 - currentColor)
4. If all nodes colored successfully → graph is bipartite

### Intuition

A graph is bipartite if we can assign two colors such that no two adjacent nodes have the same color. This is equivalent to checking if the graph contains an odd-length cycle. Using DFS coloring:
- Start with any node, assign it color 0
- All neighbors must be color 1
- Their neighbors must be color 0, and so on
- If we ever encounter a conflict (neighbor already colored with same color), the graph is not bipartite

This works for disconnected graphs too because we iterate through all nodes and start DFS from each uncolored node.

### Key Points

- Use DFS to traverse and color the graph
- `1 - color` flips between 0 and 1 (opposite color)
- Handle disconnected components by looping through all nodes
- Time: O(V + E), Space: O(V)

```java
class Solution {
    private static boolean res = true;
    
    public void dfs(int[][] graph, int node, int color, int[] coloredNodes) {
        coloredNodes[node] = color;

        int size = graph[node].length;
        for (int i = 0; i < size; i++) {
            int neighbour = graph[node][i];
            if (coloredNodes[neighbour] != -1 && coloredNodes[neighbour] == color) {
                res = false;
            }
            if(coloredNodes[neighbour] == -1) {
                dfs(graph, neighbour, 1 - color, coloredNodes);
            }
        }
        return;
    }

    public boolean isBipartite(int[][] graph) {
        int[] coloredNodes = new int[graph.length];
        int color = 0;
        res = true;
        for(int i = 0; i < coloredNodes.length; i++) {
            coloredNodes[i] = -1;
        }
        
        for(int i = 0; i < graph.length; i++) {
            if(coloredNodes[i] == -1) dfs(graph, i, color, coloredNodes);
        }
        return res;
    }
}
```

Time Complexity: O(V + E)
Space Complexity: O(V)